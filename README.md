# Image Steganography

이미지 픽셀의 최하위 비트에 텍스트를 숨기고 다시 추출하는 학습 앱입니다. 암호를 사용해 텍스트를 암호화하는 선택 기능도 있습니다.

## 주요 기능

- JPG·PNG·BMP 이미지 파일 열기
- 텍스트 입력 또는 `.txt` 파일 불러오기
- RGB 값의 최하위 비트에 문자 데이터를 순차적으로 삽입
- 이미지에서 숨긴 텍스트 추출
- 암호 사용 시 `Crypto` 클래스의 암호화·복호화 적용
- 결과 이미지를 PNG/BMP로 저장하고 텍스트를 파일로 저장하는 UI

## 프로젝트 구조

| 경로 | 역할 |
| --- | --- |
| `Steganography.sln` | Visual Studio 솔루션 |
| `Steganography/Steganography.csproj` | .NET Framework 3.5, x86 Windows 실행 파일 설정 |
| `Steganography/Program.cs` | Windows Forms 진입점 |
| `Steganography/Steganography.cs` | 이미지·텍스트 파일 처리와 숨기기/추출 이벤트 |
| `Steganography/Steganography.Designer.cs` | 폼과 컨트롤 배치 |
| `Steganography/SteganographyHelper.cs` | RGB LSB 삽입, 추출, 비트 순서 변환 |
| `Steganography/Crypto.cs` | 암호에서 키를 유도하고 IV와 암호문을 Base64로 직렬화 |
| `Steganography/Properties/` | 어셈블리 정보와 리소스·설정 |

## 개발·실행 방법

Windows 환경에서 .NET Framework 3.5 대상 Windows Forms 프로젝트를 빌드할 수 있는 Visual Studio/MSBuild 구성이 필요합니다. 현재 프로젝트는 SDK 스타일의 최신 .NET 프로젝트가 아닙니다.

1. `Steganography.sln`을 엽니다.
2. .NET Framework 3.5 대상과 x86 구성을 확인합니다.
3. 빌드 후 폼에서 이미지를 열고 텍스트를 입력합니다.
4. 암호화 여부와 암호를 정한 뒤 숨기기를 실행하고, 결과를 별도 파일로 저장합니다.
5. 저장한 이미지를 다시 열어 같은 설정으로 추출을 확인합니다.

API 키, 서버, 데이터베이스, 환경 변수는 사용하지 않습니다. macOS 환경에서는 Windows Forms 실행을 검증하지 않았습니다.

## 현재 구현의 제한

- 비암호화 삽입은 문자당 8비트만 사용합니다. 한글 등 8비트를 넘는 문자를 그대로 넣으면 손상될 수 있습니다.
- 저장 가능한 용량은 대략 `가로 × 세로 × 3 / 8`바이트에서 종료 문자 공간을 뺀 크기입니다. 현재 UI는 초과 용량을 사전에 차단하지 않습니다.
- JPEG로 다시 압축하거나 이미지를 리사이즈·보정하면 숨긴 데이터가 손상될 수 있습니다. 원본 픽셀을 보존하는 PNG/BMP 흐름을 사용해야 합니다.
- 이미지 저장 분기의 `SaveFileDialog.FilterIndex` 처리가 실제 1부터 시작하는 인덱스와 맞지 않습니다. PNG 선택 시 BMP 인코더가 사용되고 BMP 선택 시 저장 분기에 들어가지 않을 수 있습니다. 저장 동작 수정·검증이 필요합니다.
- 이미지가 선택되지 않은 상태, 잘못된 파일, 큰 이미지 처리와 같은 예외 처리가 충분하지 않습니다.
- 암호화 코드는 고정 salt와 기존 `RijndaelManaged`·`Rfc2898DeriveBytes` 기본값을 사용하며 무결성 검증을 제공하지 않습니다. 학습 예제이며 민감한 정보를 보관하는 도구로 검증되지 않았습니다.

## 출처와 검토 범위

`Crypto.cs`에는 Digiex/MCLauncher.NET 코드 참조가 남아 있습니다. 원본 주석과 출처를 유지합니다. 본 README는 솔루션·폼·알고리즘 소스를 검토한 결과이며 Windows 빌드, 암호화 호환성, 이미지 왕복 테스트가 통과했다는 의미는 아닙니다.

소스 출처·보관 브랜치·제외 범위는 [정리 기록](docs/SOURCE_MAP.md)에 있습니다.
