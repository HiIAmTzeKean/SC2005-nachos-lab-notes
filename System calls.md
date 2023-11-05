---
tags:
  - 🌱
  - OS
  - ComputerScience 
date: 02--Nov--2022
---

# System calls

```c++
//----------------------------------------------------------------------
// SyscallExit
//  This is what a user process calls to die.  The process gives this
// function one int argument indicating it's return status.  This
// function releases the address space of the process and joins on
// any children of the process that were never joined on (thus
// freeing up 'zombies').  Open files are closed.  Then the function
//      puts the exit status in a table for it's parent to receive.  A
//      semaphore is used to prevent the parent from joining before this
//      function is called.  And then the thread dies gracefully.
//
//----------------------------------------------------------------------

void SyscallExit(int processStatus)
{
  Process *processPtr;
  SList *processChildren;
  SList *processFiles;
  int id;

  DEBUG('a', "SyscallExit(), initiated by user program %s #%i.\n",
 currentThread->getName(), currentThread->pid);
  if(currentThread->space){
    delete currentThread->space;
    currentThread->space=0;
  }
  processPtr=(Process *)processes->SearchByThread(currentThread);
  processFiles=processPtr->files;
  while(!processFiles->IsEmpty()){
    processFiles->First(&id);
    SyscallClose(id);
  }  
  processChildren=processPtr->children;
  while(!processChildren->IsEmpty()){
    processChildren->First(&id);
    SyscallJoin(id);
  }
  processPtr->status=processStatus;
  processPtr->done->V(); // Allow join to get status
  currentThread->Finish();
}
```

```c++
//----------------------------------------------------------------------
// SyscallExec
//  This functions forks and executes a new program in a new address
// space.  The program is 'name'.  It starts out by converting name
// to a string in kernel memory if the threads that called exec is
// in userspace.  Then it opens the file and copies it to a new
// address space.  If either of these fail, for instance if the file
// doesn't exist, or there is no memory, -1 is returned.  A unique
// SpaceId is then obtained.  The new process is inserted into a list
// of children for the parent (this is used later in SyscallJoin and
// SyscallExit).  Then a new process entry is made in the linked list
// of processes.  The data in this entry is initialized.  Finally a new
// thread is made and the process can execute.  The SpaceId is re-
// turned to the calling function.
//
//----------------------------------------------------------------------

SpaceId SyscallExec(char *name, char *args)
{
  char *kernelName;
  AddrSpace *newAddress;
  OpenFile *executable;
  Thread *childThread;
  Process *processPtr;
  SpaceId id;

  DEBUG('a', "SyscallExec(), initiated by user program %s #%i.\n",
    currentThread->getName(), currentThread->pid);
  if(currentThread->space){
    kernelName=UserToKernelString(name);
  } else {
    kernelName=name;
  }
  
  if(!kernelName) // null pointer
    return -1;
  
  executable = fileSystem->Open(kernelName);
  
  if (executable == NULL){  // can't open file
    return -1;
  }
  if(currentThread->space)
    delete kernelName;
  id=processes->FreeId(0);
  
  newAddress=new AddrSpace(executable, id);
  delete executable;
  if(newAddress->outOfMem){
    delete newAddress;
    return -1;
  }
  childThread=new Thread("userprogram");
  childThread->pid=id;
  processPtr=(Process *)processes->SearchByThread(currentThread);
  processPtr->children->Add(new Child, id, 0);
  processes->Add(new Process, id, childThread);
  newAddress->kernelArgs=UserToKernelString(args);
  childThread->Fork(ExecProcess, (int)newAddress, FALSE);
  return id;
}
```

```c++
//----------------------------------------------------------------------
// ExecProcess
//  This is called by SyscallExec to actually run the new process.
// It is a forked kernel thread which begins by setting up it's
// address space and initializes things.  It then calls machine->Run
// which causes it to execute.
//
//----------------------------------------------------------------------

void ExecProcess(int intSpace)
{
  currentThread->space=(AddrSpace *)intSpace;
  currentThread->space->InitRegisters();
  currentThread->space->RestoreState(); // load page table register
  InitArgs(currentThread->space->kernelArgs); // init args to pass
  machine->Run();          // jump to the user progam
  ASSERT(FALSE);   // machine->Run never returns;
       // the address space exits
     // by doing the syscall "exit"
}
```

---
Links:
