# IBM Decision Optimization on Cloud R Client

IBM Decision Optimization on Cloud (DOcplexcloud) enables you to solve optimization
problems on the cloud without installing or configuring a solver. We handle
the connection so that you can jump into coding faster.

This documentation describes the R API to access the service. 

## Requirements

This library requires R 3.2 or later.

## Install the library

You can install the library from github:

    if(!require(devtools)){
        install.packages("devtools")
    }
    library(devtools)
    install_github("IBMDecisionOptimization/DOcplexcloud-R-client")

## Example

This sample is in the `demo` directory. It shows how to submit
a job with a simple model as a `.lp` file. Once the DOcplexcloud client
installed, you can run the demo in R with `demo('submit-job')`.

```R
require('docplexcloud')

job <- NULL
tryCatch({
    # uses environment variables DOCPLEXCLOUD_URL and DOCPLEXCLOUD_KEY
    # for \code{url} and \code{key} parameters
    client <- DOcplexcloudClient$new(verbose=TRUE)

    # create job, upload attachment, submit execution
    # and wait for completion
    job <- client$submitJob(addAttachment("sample_diet.lp"))
    if (job$executionStatus == "PROCESSED") {
        # Download attachment
        solution = client$getAttachment(job, "solution.json")
        # at this point, the json solution has ben parsed
        # and can be accessed using solution$CPLEXSolution
        # we can write it for future use or whatever
        write(toJSON(solution), "solution.json")
    } else {
        # An error occured
        cat("Job finished with status", job$executionStatus)
    }
}, finally = {
    if (!is.null(job))  client$deleteJob(job)
})
```

## Download samples

Samples can be downloaded from the IBM Decision Optimization [GitHub repository](<https://github.com/IBMDecisionOptimization/>).

[The predict accidents](https://github.com/IBMDecisionOptimization/DOcplexcloud-R-predict-accidents-sample)
sample is a fully featured [Shiny](<https://shiny.rstudio.com/>)
application using the IBM Decision Optimization on Cloud solve service.

## Get your IBM Decision Optimization on Cloud API key
   
 1. Register for a trial account
 
 	Register for the DOcplexcloud free trial and use it free for 30 days. See 
 	[Free trial](https://developer.ibm.com/docloud/try-docloud-free).
 
 2. Get your API key
 
    With your free trial, you can generate a key to access the DOcplexcloud API. 
    Visit the 
    [Get API key & base URL](<http://developer.ibm.com/docloud/docs/api-key) 
    page to generate the key once you've registered. This page also contains 
    the base URL you must use for DOcplexcloud.

## License

This library is delivered under the Apache License Version 2.0, January 2004 (see LICENSE).

