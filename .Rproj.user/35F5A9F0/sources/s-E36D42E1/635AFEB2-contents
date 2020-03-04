# 00. Check dependencies --------------------------------------------------

if (!require(git2r)) {
  install.packages("git2r")
}

if (!require(usethis)) {
  install.packages("usethis")
}

# 01. Initialise website --------------------------------------------------

source("dev/utils.R")
config_web()

# 02. Build and deploy website --------------------------------------------

source("dev/utils.R")
deploy_web()
