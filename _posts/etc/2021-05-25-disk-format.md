---
title: diskpart를 이용한 disk format
tags: etc
---

## Summry  

본 문서에서는 diskpart를 이용해 disk format을 진행하는 방법을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 방법

![그림](/assets/etc/diskformat/1.PNG)  

1. Ctrl + r 단축키로 실행창을 실행한 뒤 diskpart 입력하여 diskpart 실행  

2. ```list disk``` 디스크 목록 확인  

3. ```select disk number``` 포맷을 진행할 디스크 선택  

4. ```clean``` 디스크 정리  

5. ```create partition primary``` 파티션 생성  

6. ```list partition``` 파티션 목록 확인  

7. ```select partition number``` 활성화시킬 파티션을 선택  

8. ```active``` 선택한 파티션 활성화  

9. ```format fs=ntfs quick``` 파일 시스템을 ntfs 로 선택하여 디스크 포맷 또는  
```format fs=fat32 quick``` 파일 시스템을 FAT32 로 선택하여 디스크 포맷  

**완료**