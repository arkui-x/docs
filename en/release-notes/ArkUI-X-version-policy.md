# ArkUI-X Version Policy Description

## Version Format

The ArkUI-X version number takes the format of x.y.z, where x indicates the major version, y indicates the minor version, and z indicates the patch. For details versioning, see [Semantic Versioning 2.0.0](https://semver.org).

## Version Release Policy

ArkUI-X is scheduled to release a minor version every three months (each of which will increment the minor version number) and a major version every year (each of which will increment the major version number). The specific release interval is slightly adjusted based on the actual situations.

The version release criteria covers basic features, test cases, defect closure rate of version requirements, performance, stability, and basic quality requirements. For details, see [Version Quality Criteria](#VersionQualityStandard).

The version release review is led by the Project Management Committee (PMC) of ArkUI-X. A release approval needs at least three votes and no veto by the PMC members.

ArkUI-X focuses on continuous innovation and evolution. You are advised to use the latest stable version. ArkUI-X releases the roadmap every year. You are welcome to provide feedback on scenario-specific requirements through channels such as the community.

### *Version Quality Criteria*<a name ="VersionQualityStandard"></a>

| Category| Beta Version| Official Version          |
| ------------ | ------------------ | ----------------------- |
| Capability objectives        | Basic functions are stable and allow for joint commissioning, development, and verification by vendors.| Functions are stable and allow for commercial integration by vendors after having passed special tests.|
| Release period        | 3 months               | Yearly                     |
| How to obtain        | Follow the official release announcement.          | Follow the official release announcement.               |

| Quality Requirement Subcategory| Beta Version| Official Version   | Description          |
| -------------- | ------------------ | --------------- | -------------------- |
| Community access control          | Passed                | Passed             | - |
| Static check          | Passed                | Passed             | Including code specification check, compliance check, and security check report|
| Smoke test pass rate       | 100%               | 100%            | - |
| XTS test pass rate      | 100%               | 100%            | - |
| UT case pass rate       | >95%               | 100%            | - |
| Performance            | The performance requirements are met.          | The performance requirements are met.           | Based on the performance specifications of iOS and Android devices|
| Power consumption            | The power consumption requirements are met.          | The power consumption requirements are met.           | Based on the performance specifications of iOS and Android devices|
| Security            | Passed                | Passed             | - |
| Stability           | The beta baseline is met.          | The baseline is met.           | - |
| Compatibility           | The baseline is met.              | The baseline is met.           | Based on community PCS documents           |
| Known issues          | There are no security redline issues or key blocking issues.   | There are no security redline issues or key blocking issues.| - |
| Documentation            | Passed        | Passed        | - |
