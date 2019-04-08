---
layout: post
title:  "TXT 파일을 CSV로 변환하기"
date:   2019-04-08 22:50:00
categories: Python txt csv
---

 
  
   
### Python을 활용한 재미난 것들 #3
#### txt 파일을 csv 파일로 파싱하기

TXT 파일이 특정 조건에 맞춰 기록이 되어 있는 상태다.
이를 쉽게 편집하기 위해서 csv 파일로 저장하는 예제이다.


``` python

#-*-coding:utf-8
import sys, io, os
import pandas as pd

# output 값이 한글인 경우, encoding을 위한 설정
sys.stdout = io.TextIOWrapper(sys.stdout.detach(), encoding = 'utf-8')
sys.stderr = io.TextIOWrapper(sys.stderr.detach(), encoding = 'utf-8')

# 아래 폴더에 대상 파일이 있다고 가정
os.chdir("D:/python")

file = open('origin.txt', mode='rt', encoding='utf-8')
data = file.readlines()

# 행 기준이 | 으로 나뉜다고 가정
fLabel = data[0].split('|')

fList = []

for i in range(1,len(data)):
    # 열 기준이 \ 으로 나뉜다고 가정
    fList.append(tuple(data[i].split('\')))


df = pd.DataFrame.from_records(fList, columns=fLabel)

# encoding을 utf-8 으로 하니 오류가 발생했다. window 기본 인코딩으로 변경하니 정상!
df.to_csv('converted.csv', encoding='ms949', index=True)


```


* encoding은 UTF-8 으로 통일했는데, CSV 파일에서 깨져보이는 현상이 발생했다. 구글링 결과로 ms949로 변경하니 정상이다.
* Pandas는 갓갓. 대충 이런 기능 있지 않을까 검색하면 모두 지원하고 있다!! 
