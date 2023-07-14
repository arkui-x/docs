# Supported Commands for PR Comments in ArkUI-X

| Command| Mandatory/Optional| Scenario                                                    | Role to Trigger Command         |
| ------------ | -------- | ------------------------------------------------------------ | --------------------- |
| start build  | **Mandatory**| Trigger build test.                                            | PR creator<br>Committer|
| stop build   | Optional    | Stop build test.                                            | PR creator             |
| submit       | Optional    | Manually trigger the merging if automatic merging is abnormal. The PR can be merged after the pass conditions are met.<br>Pass conditions: The arkui-x_ci test is passed, and the PR is allowed for merging.| Anyone               |
| check dco    | Optional    | Manually trigger the DCO check after updating the DCO information in the case of DCO check failure.<br>Pass conditions: DCO is signed, and Signed-off-by information is included in all commits of the PR. For details, see [DCO Verification](./FAQ.md). | PR creator             |
| static-check | Optional    | Manually trigger the static check If the static check fails. The static check is used only for independent verification and does not affect the final build result.| PR creator             |
