# Deploy shiny app using custom package

-   User `renv` in your app project
-   Generate a [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) in GitHub
-   Modify the `.Rprofile` (is in your project directory) to show this:

```{r}
source("renv/activate.R")
local({
  r <- getOption("repos")
  r["CRAN"] <- "https://cran.rstudio.com/"
  r["mycompany"] <- "https://github.com/<your_user>/<your_repo>/"
  options(repos = r)
})
```

-   Within your project, open the R terminal and install your package:

```{r}
devtools::install_github("<my_user>/<my_repo>",
	auth_token = "<paste_your_token_here>",
	upgrade = "never") #this is optional, 
# but since you are using renv, you may want to conserve 
# your environment
```

This will install your package in the `renv`

**IMPORTANT**: don't include the previous line in the source code you're going to upload to shiny.io or similar [Read about it](https://groups.google.com/g/shinyapps-users/c/uX22Tu_veOM)

-   Publish your app.

After you've done this, *rsconnect* should be able to read your packages from your source code and upload them from your *renv*. Actually, the first time shiny.io build my package, there was a time out and didn't upload the app. But, it installed my custom package in its environment (i guess...) and the second time I tried to uploaded it worked.

**NOTE**: In my case, my package was in a public github repository. Haven't tried with private repos, but I know there's an option in *shiny.io* that connects the server to the private repositories. I think you will also need to configure your personal access token so it can be use to read private repos. 