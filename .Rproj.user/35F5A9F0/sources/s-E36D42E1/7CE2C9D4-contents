#' Check dependencies

#' Configuration
#' 
#' Prepare publish directory for deployment.
#' 
#' @param publish_dir directory for published website, default is "public"
#' @param remote_repo remote repository for website. Note that the repo has to be 'username/username.github.io'
#' 
#' @importFrom usethis ui_todo ui_done ui_info ui_code_block
#' @importFrom git2r init remote_add add commit

config_web <- function(publish_dir = "public", remote_repo = rstudioapi::showPrompt(title = "GitHub repository", message = "Please provide your GitHub repository (currently only HTTPS protocol is supported)", default = "https://github.com/username/username.github.io.git")) {
  usethis::ui_todo("Preparing publish directory")
  
  if (!dir.exists(publish_dir)) {
    dir.create(publish_dir)
  }
  
  if (dir.exists(".git")) {
    usethis::use_git_ignore(publish_dir)
  }
  
  if (!file.exists(paste0(publish_dir, "/.nojekyll"))) {
    file.create(paste0(publish_dir, "/.nojekyll"))
  }
  
  usethis::ui_todo("Preparing git in publish directory")
  if (!dir.exists(paste0(publish_dir, "/.git"))) {
    git2r::init(publish_dir)
  }
  
  git2r::remote_add(publish_dir, "website", url = remote_repo)
  git2r::add(publish_dir, path = ".")
  git2r::commit(publish_dir, message = "Initialize website")
  
  usethis::ui_done("Publish directory is ready")
}

#' Build and deploy
#' 
#' Render R Markdown, run hugo command, commit changes, and finally push website into GitHub repository.
#' 
#' @param publish_dir directory for published website, default is "public"
#' @param commit_message commit message
#' 
#' @importFrom usethis ui_todo ui_done
#' @importFrom blogdown build_site
#' @importFrom git2r add commit cred_user_pass push
#' 
#' @export
deploy_web <- function(publish_dir = "public", commit_message = rstudioapi::showPrompt(title = "Commit message", message = "Please write your commit message", default = "Update contents")) {
  usethis::ui_todo("Building site")
  blogdown::build_site()
  usethis::ui_done("Site is successfully built")
  
  usethis::ui_todo("Commiting changes")
  git2r::add(publish_dir, path = ".")
  git2r::commit(publish_dir, message = commit_message)
  usethis::ui_done("Changes are successfully commited")
  
  usethis::ui_todo("Pushing changes to GitHub")

  cred <- git2r::cred_user_pass(
    username = rstudioapi::askForPassword("GitHub username"), 
    password = rstudioapi::askForPassword("GitHub password")
  )
    
  git2r::push(publish_dir, "website", "refs/heads/master", credentials = cred, set_upstream = TRUE)
  usethis::ui_done("Done! Wait a few seconds and check your website")
}
