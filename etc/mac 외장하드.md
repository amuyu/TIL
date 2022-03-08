

```
# 1. usb 명, identifier 명 알아내기
diskutil list 
# 2. usb unmount 하기
sudo diskutil unmount /dev/<IDENTIFIER 명> 
# 3. usb에 새로운 디렉토리 생성하기. 여기서 $NAME은 USB명. 
sudo mkdir /Volumes/$NAME 
# 4. usb mount 하기
sudo mount -w -t msdos /dev/<IDENTIFIER 명> /Volumes/$NAME 
# 5. 쓰기가 되는지 검증용으로 샘플 파일 작성하기
touch /Volumes/$NAME/tmp.txt 
# 6. usb에 저장된 파일 리스트를 불러와서 방금 작성한 파일이 제대로 써졌는지 더블체크하기 
ls -al /Volumes/$NAME 
# 7. 파일 옮기는 작업 종료 후 unmount하기
sudo diskutil unmount <IDENTIFIER 명>
ex) sudo diskutil unmount /dev/disk2s1
```

https://nurilee.com/2020/04/01/mac-usb-read-only-solving-even-after-formatting/