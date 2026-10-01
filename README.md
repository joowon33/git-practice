# Git 실습용 계산기

함수와 import만 사용하는 작은 Python 프로그램입니다. 숫자 입력에는 `float`를 사용합니다. 실습할 때는 숫자를 입력하세요. 별도 패키지 설치는 필요 없습니다.

```powershell
python main.py
```

`python` 명령이 없으면 `py main.py`를 사용하세요.

| 파일 | 역할 |
| --- | --- |
| `main.py` | 숫자 두 개를 입력받고 결과 출력 |
| `calculator.py` | 계산 함수 |
| `messages.py` | 시작 안내 문구 |

| 준비된 브랜치 | 들어 있는 기능 |
| --- | --- |
| `main` | 덧셈과 뺄셈 |
| `feature/multiply` | main에서 나뉘어 곱셈 추가 |
| `feature/greeting` | main에서 나뉘어 안내 문구 개선 |

두 기능 브랜치는 서로 독립적이며, 아직 main에 합쳐지지 않았습니다. 완성된 나눗셈은 제공하지 않습니다. 직접 추가해 보세요.

## 1. 저장소 clone하기

Git, Python, VS Code와 GitHub 계정을 준비하세요. 아래 명령은 Windows PowerShell 기준입니다. 코드 블록을 단계별로 실행하세요.


```powershell
git clone ./git-practice https://github.com/downtown1629/git-practice.git
cd git-practice
```

이후 터미널의 현재 폴더는 항상 `git-practice`입니다.

```powershell
git status
git branch -a
git log --oneline --graph --all
python main.py
```

clone은 파일과 커밋 이력을 가져옵니다. 처음 체크아웃되는 로컬 브랜치는 main 하나이며, 나머지는 `remotes/origin/feature/...`로 보입니다. `origin`은 clone한 원본의 주소를 가리키는 이름입니다.

## 2. 브랜치를 바꾸며 코드 비교하기

먼저 두 기능 브랜치의 로컬 브랜치를 만듭니다. `git branch`는 브랜치를 만들지만 현재 브랜치를 바꾸지는 않습니다.

```powershell
git branch feature/multiply origin/feature/multiply
git branch feature/greeting origin/feature/greeting
git switch feature/multiply
python main.py
git diff main..feature/multiply -- calculator.py main.py
```

곱셈 결과가 추가됩니다. VS Code에서 `calculator.py`와 `main.py`도 확인하세요.

```powershell
git switch feature/greeting
python main.py
git diff main..feature/greeting -- messages.py
git switch main
```

안내 문구가 바뀌고 곱셈은 사라집니다. switch하면 선택한 브랜치의 파일로 작업 폴더가 바뀝니다. 이 단계에서는 파일을 수정하지 않습니다.

## 3. 내 GitHub에 세 브랜치 올리기

GitHub에 로그인하고 **New repository**로 새 저장소를 만드세요.

- 소유자: 본인 계정
- 이름: `git-my-practice`
- 공개 여부: 원하는 대로 선택
- README, .gitignore, license 초기화: 모두 선택하지 않기

이미 로컬에 커밋이 있으므로 GitHub 저장소는 빈 상태로 만듭니다. 생성 후 표시되는 HTTPS 주소를 복사하세요.

아래 `YOUR_ID`는 **자신의 GitHub 아이디로 바꿔야 합니다**. 저장소 이름도 다르게 만들었다면 바꾸세요.

```powershell
git remote -v
git remote set-url origin https://github.com/YOUR_ID/git-my-practice.git
git remote -v
git push -u origin main
git push -u origin feature/multiply
git push -u origin feature/greeting
```

`set-url`은 기존 origin의 목적지를 내 GitHub로 바꿉니다. 커밋이나 파일을 바꾸지는 않습니다. `push`는 로컬 커밋을 GitHub에 보내고, `-u`는 해당 로컬 브랜치와 원격 브랜치를 연결합니다.

처음 push할 때 인증 창이 뜨면 GitHub 로그인을 진행하세요. HTTPS 인증에 일반 계정 비밀번호를 입력하는 방식은 지원되지 않습니다. Git Credential Manager의 브라우저 로그인이나 개인 액세스 토큰을 사용합니다.

GitHub 페이지를 새로고침하고 브랜치 선택 메뉴에서 세 브랜치가 모두 있는지 확인하세요. 아직 기능 브랜치를 main으로 합치지는 마세요.

## 4. 새 브랜치에서 기존 기능 합치기

main에서 실습용 브랜치를 나눕니다.

```powershell
git switch main
git switch -c practice/calculator
git branch
git merge --no-ff feature/multiply -m "곱셈 브랜치 병합"
python main.py
git merge --no-ff feature/greeting -m "안내 브랜치 병합"
python main.py
git log --oneline --graph --all
```

`switch -c`는 현재 커밋에서 새 브랜치를 만들고 이동합니다. merge는 지정한 브랜치의 작업을 **현재 브랜치**로 합칩니다. 여기서는 practice/calculator에 곱셈과 안내 기능이 들어오며, main은 그대로입니다.

`--no-ff`는 합친 지점을 병합 커밋으로 남겨 그래프에서 확인하기 쉽게 합니다. 준비된 두 기능은 서로 다른 부분을 수정하므로 충돌 없이 합쳐집니다.

## 5. 나눗셈을 직접 추가하고 커밋 두 개 만들기

먼저 작성자 정보를 설정합니다. 이미 본인의 정보로 설정했다면 생략할 수 있습니다. 예시 이름과 이메일은 본인의 값으로 바꾸세요. 이 설정은 이 저장소에만 적용됩니다.

```powershell
git config user.name "내 이름"
git config user.email "내 GitHub 이메일"
```

**첫 번째 수정:** VS Code에서 `calculator.py` 맨 아래에 다음 함수를 추가하고 저장합니다.

```python
def divide(a, b):
    return a / b
```

수정 → 확인 → stage → commit 순서로 진행합니다.

```powershell
git status
git diff
git add calculator.py
git diff --cached
git commit -m "나눗셈 함수 추가"
```

`diff`는 아직 stage하지 않은 수정을 보여줍니다. `add`는 다음 커밋에 담을 내용을 stage합니다. `diff --cached`는 커밋에 담길 내용을 보여줍니다. `commit`은 그 내용을 로컬 이력으로 저장합니다. 아직 GitHub에는 보내지 않았습니다.

**두 번째 수정:** `main.py`의 `main()` 함수 안에서 곱셈 출력 바로 아래에 다음 코드를 추가하고 저장합니다. 같은 들여쓰기(공백 4개)를 유지하세요.

```python
    if b == 0:
        print("나눗셈: 0으로 나눌 수 없습니다.")
    else:
        print("나눗셈:", calculator.divide(a, b))
```

```powershell
python main.py
git diff
git add main.py
git diff --cached
git commit -m "나눗셈 결과 출력"
git status
git log --oneline -5
```

숫자 8과 2를 입력하면 나눗셈 결과는 4.0입니다. 다시 실행하여 8과 0도 확인하세요. 작업 폴더에 미커밋 변경이 없는지 확인합니다.

## 6. push하고 PR 만들기

```powershell
git push -u origin practice/calculator
```

GitHub의 본인 저장소에서 **Pull requests → New pull request**로 들어갑니다.

- base: `main` (변경을 받을 브랜치)
- compare: `practice/calculator` (변경을 보낼 브랜치)
- 제목: `계산기 기능 추가`
- 설명: `곱셈, 안내 문구, 나눗셈을 추가했습니다. 8과 2, 8과 0으로 실행했습니다.`

Files changed에서 변경 파일을 살펴본 뒤 **Create pull request**를 누릅니다. push는 브랜치를 업로드하고, PR은 그 변경을 검토하여 다른 브랜치에 합치자고 제안합니다. PR을 만들기만 하면 main은 바뀌지 않습니다.

## 7. GitHub에서 합치고 로컬 main 갱신하기

PR의 변경 내용을 검토한 뒤 **Merge pull request → Confirm merge**로 합칩니다. 실습에서는 병합 방식을 **Create a merge commit**으로 선택하세요. 자신의 저장소이므로 직접 합칠 수 있습니다.

GitHub에서 main이 바뀌어도 로컬 main은 자동으로 바뀌지 않습니다.

```powershell
git switch main
python main.py
git pull --ff-only origin main
python main.py
git log --oneline --graph --all
```

pull 전에는 덧셈·뺄셈만 있고, pull 후에는 모든 기능이 보입니다. `pull`은 원격의 새 커밋을 가져와 현재 브랜치에 반영합니다. `--ff-only`는 로컬이 원격 이력을 그대로 따라갈 수 있을 때만 갱신합니다.

완료 확인: 내 GitHub에 세 초기 브랜치와 실습 브랜치가 있고, PR이 Merged 상태이며, 로컬 main에서도 곱셈과 나눗셈이 실행됩니다.

## 강사용 배포 메모

`git-practice`가 원본입니다. 교육생은 clone한 `git-my-practice`에서 작업합니다. 폴더로 전달할 때는 숨김 폴더 `.git`도 함께 전달하세요.

강사의 GitHub로 배포한다면 먼저 빈 저장소를 만든 뒤, 원본 폴더에서 아래 명령을 실행하세요. `TEACHER_ID`는 실제 아이디로 바꿉니다.

```powershell
git remote add origin https://github.com/TEACHER_ID/git-practice.git
git push -u origin main
git push -u origin feature/multiply
git push -u origin feature/greeting
```

교육생은 이 주소를 clone한 뒤 2단계부터 진행합니다. GitHub 게시와 PR 생성은 실제 계정에서 진행할 실습입니다.

공식 참고: [기존 로컬 저장소를 GitHub에 올리기](https://docs.github.com/en/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github), [PR 만들기](https://docs.github.com/en/pull-requests/how-tos/create-pull-requests/creating-a-pull-request), [GitHub HTTPS 인증](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls).
