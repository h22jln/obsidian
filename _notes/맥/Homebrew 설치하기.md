``/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"``

입력 후 두 문장 추가로 입력 (m1 이상)

`echo 'eval $(/opt/homebrew/bin/brew shellenv)' >> /Users/본인 홈 이름/.zprofile
``eval $(/opt/homebrew/bin/brew shellenv)``
*\*터미널에 나와있는 그대로 복사하면 됨*

``brew help``로 실행 후
``brew --version`` 으로 잘 설치되었는지 확인 
