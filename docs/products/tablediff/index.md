# TableDiff 개요

TableDiff는 **데이터베이스 간의 데이터 일관성을 검증**하고 **변경 사항을 효과적으로 비교 분석**하기 위해 설계된 강력한 도구입니다. 특히 CDC(Change Data Capture) 솔루션의 검증 과정에서 원본 데이터베이스와 대상 데이터베이스 간의 차이를 정밀하게 식별하는 데 활용됩니다.

주요 기능:

* 두 개의 SQL ResultSet을 기반으로 정렬된 스트리밍 비교를 수행합니다.
* 비교 결과를 `Same`, `Change`, `OnlyInA`, `OnlyInB` 네 가지 카테고리로 분류하여 제공합니다.
* 다양한 데이터베이스 환경 (Oracle 등)을 지원하며, Docker를 통한 테스트 환경 구축이 용이합니다.

TableDiff 프로젝트는 내부 GitLab에서 관리되고 있습니다. 자세한 내용은 다음 링크를 참조하십시오:
:fontawesome-brands-gitlab: [TableDiff GitLab Repository](https://rnd.iarkdata.com/gitlab/ARK/CDC/tablediff)