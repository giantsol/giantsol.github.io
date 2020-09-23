---
title: "즐겨쓰는 ADB 커맨드들"
tags:
  - android
---

ADB(Android Debug Bridge)는 "USB 디버깅"이 켜져있는 기기들에 대해 여러 디버깅을 가능하게 해주는 도구이다.
웬만하면 Android Studio로 커버가 가능하지만, 몇 가지 adb를 직접 쓰는 경우들에 대해 내가 자주 쓰는 기능들만 적어본다.

## 1. adb devices

간단하게 연결된 기기들의 리스트를 보여준다.

## 2. adb install abc.apk

간혹 테스트용으로 컴퓨터로 다운받아둔 apk를 설치하고 싶을 때 사용한다.
-d, -t 같은 여러 플래그가 있으므로 `adb install --help`로 다른 플래그들도 참고!

## 3. adb shell screencap /sdcard/temp.png

현재 화면을 공유하기 위해 애용하는 기능으로, 현재 핸드폰의 스샷을 찍는다.
위 커맨드의 경우 기기의 /sdcard/temp.png라는 경로로 저장된다.

## 4. adb shell screenrecord /sdcard/temp.mp4

3번과 유사하지만 얘는 화면 녹화다. ctrl-c를 누르면 녹화가 종료된다.

## 5. adb pull /sdcard/temp.mp4

3번 또는 4번으로 저장된 데이터를 밖으로(컴퓨터로) 끄집어낼 수 있다.
화면 녹화를 한 후 다른 사람들에게 공유하려고 할 때 자주 사용한다.

## 6. adb shell dumpsys

더 깊은 디버깅을 할 때 자주 사용하는 기능으로, 기기의 여러 상태들(e.g. 액티비티, 서비스 등)에 대한 로그를 살펴볼 수 있다.
엄청 긴 리스트가 나오므로 `adb shell dumpsys >| temp.txt` 식으로 해서 파일로 보는게 편하다.

`adb shell dumpsys -l`을 하면 개별로 살펴볼 수 있는 상태들의 리스트가 나온다.
나는 특히 `adb shell dumpsys activity`로 액티비티들의 상태와, 액티비티와 연관된 Task의 상태 등을 볼 때 자주 사용한다.
