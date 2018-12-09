# Alias
BIG_PULL_REQUEST_LINES = 1000
APP_FILES = /^(app|lib)/
TEST_FILES = /^(features|spec|test)/
has_app_changes = !git.modified_files.grep(APP_FILES).empty?
has_test_changes = !git.modified_files.grep(TEST_FILES).empty?

## Check label and body of PR.
has_label_wip = github.pr_title.match(/wip/i) || github.pr_labels.include?('wip') || github.pr_labels.include?('Wip') || github.pr_labels.include?('WIP')
has_pr_body_no_check = github.pr_body.match(/- \[ \]/)

# Request changes and Attention
## TODO in code.
## ref. https://github.com/hanneskaeufler/danger-todoist
todoist.message = "TODO対応してくれないの？:cry:"
todoist.warn_for_todos
todoist.print_todos_table

## WIP
fail('まだWIPだからmergeできないの！') if has_label_wip

## TODO in PR body.
fail('「[ ] チェックリスト」全部終わらせるなの！') if has_pr_body_no_check

## Attention to code
warn('1000行以上の修正は、危ないかもだけどー？') if git.lines_of_code > BIG_PULL_REQUEST_LINES
warn('appのコードを修正してるけど、testは書かないのー？') if has_app_changes && !has_test_changes

## Attention to a PR without any changes
if git.modified_files.empty? && git.added_files.empty? && git.deleted_files.empty?
  message('何も修正してないかもだけどー？')
end

# Procedure for re-executing CircleCI
message(
  "@#{github.pr_author}\n\n"\
  "失敗した時は、指摘内容を修正して\n"\
  "<a href='https://circleci.com/workflow-run/" + ENV['CIRCLE_WORKFLOW_ID'] + "'>https://circleci.com/workflow-run/" + ENV['CIRCLE_WORKFLOW_ID'] + "</a>\n"\
  "から「Rerun from failed」を実行してなの！"
) if !violation_report[:errors].empty?

# LGTM pic
# ref. https://github.com/leonhartX/danger-lgtm
lgtm.check_lgtm
