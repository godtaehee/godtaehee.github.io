---
layout: post
title:  "Git bash쉘과 내장 쉘에서의 hash-object의 해시값이 다름"
subtitle: "Git bash쉘과 내장 쉘에서의 hash-object의 해시값이 다름에 대해서 다룹니다."
categories: development
tags: git-issue
comments: true
---
[원문](https://stackoverflow.com/questions/42537612/different-hashes-when-using-git-hash-object-stdin-in-bash-and-cmd-shells)

# 질문

![img-39](https://user-images.githubusercontent.com/44861205/124295907-599c7280-db94-11eb-8346-7839c75f6708.png)

`echo "Apple Pie" | git hash-object --stdin`를 통해 echo "Apple Pie"라는 데이터를 가진 hash-object를 생성했는데 Git Bash쉘에서와 자기 컴퓨터에 내장되어있는 기본 Terminal에서의 해시값이 다른데 왜 다른건지 모르겠다는 질문인데 똑같은 레포에서 다른 쉘로 hash-object를 생성할때 내용보다 더 중요하게 작용하는 어떤 요소가 깃의 시스템에 내장되어있는지에 대해서도 물어보고있는데 결론은 그냥 그런요소는 없고 내용이 실제로 달랐다.

# 답변

![img-40](https://user-images.githubusercontent.com/44861205/124295918-5b663600-db94-11eb-9d06-db0cbf789963.png)


Git bash쉘 에서는 echo "Apple Pie"는 `Apple Pie\n`을 출력하고  
기본 Terminal에서는 "Apple Pie"\\r\\n을 출력 하기때문에

컨텐츠 내용이 다르니 당연히 해쉬값이 다를수밖에없다라는 답변이였다.