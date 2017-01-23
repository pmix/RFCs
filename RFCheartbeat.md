# RFCnnnn
The RFC number will be provided upon submission.

## Title
Provide a mechanism by which an application may register a period of time as a "heartbeat".  When active, the application must issue another heartbeat within the specified period of time.  If the application fails to do so, the resource manager should consider the application to be hanging and take action to kill the job step.

## Abstract
Inspired by [io-watchdog](https://github.com/grondo/io-watchdog), this proposes a standardized interface enabling an application to register a heartbeat period with the resource manager.  When enabled, the application must issue a subsequent heartbeat before the time period from the first heartbeat expires.  If it fails to do so, the resource manager should consider the job to be hanging and take action specified by the application.  This feature is useful to automatically detect hanging jobs and remove them so jobs do not sit idle on the system.

## Labels
[EXTENSION]

## Action
The RFC administrators will mark the RFC with one of the following labels, along with the date of the indicated action:

* [APPROVED] - upon final approval of the RFC. This will include
  the date the RFC was accepted, the date the code PR was committed to the master, and the commit SHA.
* [WITHDRAWN] - proposal has been withdrawn from consideration. This typically
   will be done when alternative proposals yield a preferred solution.
* [REJECTED] - community decided not to commit the proposed changes.

## Copyright Notice
This document is subject to all provisions relating to code contributions to the PMIx community as defined in the community's [LICENSE](https://github.com/pmix/RFCs/tree/master/LICENSE) file. Code Components extracted from this document must include the License text as described in that file.

## Description
Parallel applications sometimes deadlock or block indefinitely without making progress.  This problem can arise from race conditions in the application code as well as bugs in system software or hardware.  Such jobs waste compute resources, and they are especially wasteful on large-scale systems.  This proposal defines an interface to enable the system to automatically detect and take action with such jobs.  It is inspired by [io-watchdog](https://github.com/grondo/io-watchdog).

This proposes a standard interface to enable an application to register a heartbeat period with the resource manager.  It defines the following PMIx keys in include/pmix_common.h:

  * define PMIX\_HEARTBEAT\_PERIOD "pmix.heartbeat.period" - (uint32\_t) set/reset number of seconds in current heartbeat period
  * define PMIX\_HEARTBEAT\_ACTION  "pmix.heartbeat.action"  - (char*) comma-delimited list of pre-defined actions

The heartbeat period is specified in seconds.  The heartbeat feature is disabled by default.  It is enabled when the application sets the heartbeat period to a positive integer value.  The heartbeat period shall be reset if the application specifies a new value before the current heartbeat period expires.  The application may disable the heartbeat feature by setting the period to 0.

If the heartbeat period expires, the resource manager can consider the job to be hanging, and it should execute other actions as specified by the application.  The default action is to kill the job step.

The user may specify actions by additionally setting the PMIX\_HEARTBEAT\_ACTION key.  The value for this key is a comma-delimited list of pre-defined keywords including: "pmix.email", "pmix.kill".  With "pmix.email", the resource manager will send an email to the user owning the job to notify the user that the heartbeat expired.  With "pmix.kill", the resource manager shall kill the current job step.  The PMIx implementation is free to define other keywords.

The following example illustrates usage of these keys:

  ```c
  pmix_proc_t myproc;
  int rc;
  char *tmp;
  pmix_value_t value;

  /* init us */
  if (PMIX_SUCCESS != (rc = PMIx_Init(&myproc))) {
      fprintf(stderr, "Client ns %s rank %d: PMIx_Init failed: %d\n", myproc.nspace, myproc.rank, rc);
      exit(0);
  }
  fprintf(stderr, "Client ns %s rank %d: Running\n", myproc.nspace, myproc.rank);

  /* only need myproc.rank == 0 to set the heartbeat */
  if (myproc.rank == 0) {
    /* specify email and kill actions */
    value.type = PMIX_STRING;
    value.data.string = "pmix.email,pmix.kill";
    if (PMIX_SUCCESS != (rc = PMIx_Put(PMIX_LOCAL, tmp, &value))) {
        fprintf(stderr, "Client ns %s rank %d: Failed to set heartbeat actions: %d\n", myproc.nspace, myproc.rank, rc);
        exit(0);
    }
    free(tmp);

    /* set heartbeat to one hour (3600 seconds) */
    value.type = PMIX_UINT32;
    value.data.uint32 = 3600;
    if (PMIX_SUCCESS != (rc = PMIx_Put(PMIX_LOCAL, tmp, &value))) {
        fprintf(stderr, "Client ns %s rank %d: Failed to set heartbeat period: %d\n", myproc.nspace, myproc.rank, rc);
        exit(0);
    }
    free(tmp);
  }
  ```
  
## Protoype Implementation
Provide a reference link to the accompanying Pull Request (PR) against the PMIx master repository. If the prototype implementation has been tested against an appropriately modified resource manager and/or client program, then references to those prototypes should be provided. Note that approval of any RFC will be far more likely to happen if such validation has been performed!

## Author(s)
Adam Moody  
Lawrence Livermore National Laboratory  
Github: adammoody  
