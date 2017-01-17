# RFCnnnn
The RFC number will be provided upon submission.

## Title
Provide a mechanism by which an application can query the resource manager to obtain the time remaining in its allocation.

## Abstract
Inspired by [libyogrt](https://github.com/LLNL/libyogrt), this proposes a standardized interface enabling an application to query the resource manager for the remaining time in its allocation.  Such information is useful in order for an application to shut down gracefully before its allocation ends.

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
Time-shared systems impose maximum run times on applications when assiging resource allocations.  For applications to shut down gracefully, e.g., to write a checkpoint before termination, it is necessary for applications to periodically query the resource manager for the time remaining in the allocation.  This is especially true on systems for which allocation times may be shortened or lengthened from the original time limit.  Many resource managers provide APIs to dynamically obtain this information (e.g., slurm\_get\_rem\_time() in SLURM), but each API is specific to the resource manager.

This proposal defines new PMIx attributes and semantics to provide a uniform interface to obtain the time remaining in a job allocation.  The semantics defined here are inspired by experiences from the development and use of the "Your One Get Remaining Time Library" [libyogrt](https://github.com/LLNL/libyogrt).

  New standard attributes are defined in pmix\_common.h:

  * define PMIX\_TIME\_REMAINING "pmix.time.remaining" - (uint32\_t) get number of seconds remaining in allocation
  * define PMIX\_TIME\_INTERVAL  "pmix.time.interval"  - (uint32\_t) set/get the number of seconds for which the PMIx client library may cache the remaining time without issuing a new query to the server

  To mitigate problems due to distributed clocks, only the process having rank 0 as returned in PMIx\_Init may execute PMIx\_Put and PMIx\_Get calls using these keys.  The PMIx client library shall return an error for all other ranks.

  For efficiency, the PMIx client library may cache the most recent query to the resource manager and estimate the remaining time using the current system time and simple arithmetic.  The PMIx client library may cache the result of its most recent query for the number of seconds as given by the PMIX\_TIME\_INTERVAL value.  After this interval, the PMIx must issue a new query to the resource manager.  An application may adjust the cache interval by issuing a PMIx\_Put of a new PMIX\_TIME\_INTERVAL value.

  The following example illustrates usage of these keys:

  ```c
  pmix_proc_t myproc;
  int rc;
  pmix_value_t value;
  pmix_value_t *val = &value;
  size_t n;

  /* init us */
  if (PMIX_SUCCESS != (rc = PMIx_Init(&myproc))) {
      fprintf(stderr, "Client ns %s rank %d: PMIx_Init failed: %d\n", myproc.nspace, myproc.rank, rc);
      exit(0);
  }
  fprintf(stderr, "Client ns %s rank %d: Running\n", myproc.nspace, myproc.rank);

  /* only valid for myproc.rank == 0 to make time remaining calls */
  if (myproc.rank == 0) {
    /* get current interval for caching time remaining query */
    if (PMIX_SUCCESS != (rc = PMIx_Get(&myproc, PMIX_TIME_INTERVAL, NULL, 0, &val))) {
        fprintf(stderr, "Client ns %s rank %d: PMIx_Get time interval failed: %d\n", myproc.nspace, myproc.rank, rc);
        goto done;
    }
    uint32_t time_interval = val->data.uint32;
    PMIX_VALUE_RELEASE(val);
    fprintf(stderr, "Client %s:%d seconds to cache time remaining query %d\n", myproc.nspace, myproc.rank, time_interval);

    /* set new interval for caching time remaining query to be 60 seconds */
    value.type = PMIX_UINT32;
    value.data.uint32 = 60;
    if (PMIX_SUCCESS != (rc = PMIx_Put(PMIX_LOCAL, PMIX_TIME_INTERVAL, &value))) {
        fprintf(stderr, "Client ns %s rank %d: PMIx_Put time interval failed: %d\n", myproc.nspace, myproc.rank, rc);
        goto done;
    }
    if (PMIX_SUCCESS != (rc = PMIx_Commit())) {
        fprintf(stderr, "Client ns %s rank %d: PMIx_Commit failed: %d\n", myproc.nspace, myproc.rank, rc);
        goto done;
    }

    /* get seconds remaining in allocation */
    if (PMIX_SUCCESS != (rc = PMIx_Get(&myproc, PMIX_TIME_REMAINING, NULL, 0, &val))) {
        fprintf(stderr, "Client ns %s rank %d: PMIx_Get time remaining failed: %d\n", myproc.nspace, myproc.rank, rc);
        goto done;
    }
    uint32_t secs_remaining = val->data.uint32;
    PMIX_VALUE_RELEASE(val);
    fprintf(stderr, "Client %s:%d seconds remaining in allocation %d\n", myproc.nspace, myproc.rank, secs_remaining);
  }
  ```
  
## Protoype Implementation
Provide a reference link to the accompanying Pull Request (PR) against the PMIx master repository. If the prototype implementation has been tested against an appropriately modified resource manager and/or client program, then references to those prototypes should be provided. Note that approval of any RFC will be far more likely to happen if such validation has been performed!

## Author(s)
Adam Moody
Lawrence Livermore National Laboratory
Github: adammoody
