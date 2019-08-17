
# guide
첫 줄에 50자 이내로 커밋 내용을 요약

50자 요약으로 커밋을 설명하기에 부족하면 두번째 줄을 비우고 세번째 줄부터 상세 내용을 적는다. 세번째 줄부터는 한 줄을 72자 이내로 제한
```log
UserCommand 추상 기반 클래스를 추가한다

다음 명령 클래스가 UserCommand 클래스를 상속하도록 변경한다.
- CreateUserWithEmail
- ChangeUsername
- ChangeEmail
```

# Fabric commit
- a short and descriptive subject line that is 55 characters or fewer, followed by a blank line,
- a change description with the logic or reasoning for your changes, followed by a blank line,
- a Signed-off-by line, followed by a colon (Signed-off-by:), and
- a Change-Id identifier line, followed by a colon (Change-Id:). Gerrit won’t accept patches without this identifier.

## well-formed
- what the change does,
- why you chose that appoach, and
- how you know it works - for example, which tests you ran.

# enlginsh expression
improve, add, update, remove, use, fix, upgrade, typo


# example



# ref
[Visual Studio Code를 사용해 Git 커밋 메시지 작성하기](http://blog.weirdx.io/post/53799)
[How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/) 
[Submitting a Change to Gerrit](https://hyperledger-fabric.readthedocs.io/en/latest/Gerrit/changes.html)