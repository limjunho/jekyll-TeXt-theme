---
title: Github 저장소에서 특정 branch만 clone하기
tags: Git
---

**코드를 merge(병합)하였을 때 발생하는 conflict를 VSC에서 손쉽게 해결하는 방법**  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### conflict해결하기  

vscode에서는 conflict가 발생한 부분을 배경 컬러로 알려주고 손쉽게 해결까지 가능하여 매우 편리하다.  

**아래 이미지 최상단의 Accept ~ Change를 클릭하면 변경사항을 적용해주고 git이 만든 코드도 지워준다.**  
![그림1](/assets/Git/VSC_conflict해결/1.PNG)  

### Accept ~ Change 4가지 기능  

**Accept Current Change -> 헤드 부분을 적용**  
**Accept Incoming Change -> 변경된 부분을 적용(병합 대상이 된 브랜치의 내용으로 변경)**  
**Accept Both Change -> 둘다 적용(말그대로 헤드와 변경된 부분 둘다 남겨준다.)**  
**Compare Change -> 컨플릭트가 난 부분을 좀 더 보기쉽게 보여준다.**  