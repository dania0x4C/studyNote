vi.mock('모듈_경로', () => ({
  함수명: vi.fn().mockImplementation(() => 반환값),
}));

0. vi.mock(): 목 데이터를 생성
1. vi.fn(): 가짜 함수 생성기 => 반환 값
	1. 다음의 방식으로 가짜 함수의 반환 값을 정함
2. vi.mock(axios): axios 통신 반환 값 자체의 목 데이터를 생성 

**beforeEach(() => { ... })**

• 각 테스트 **실행 전에 항상 이 함수**가 실행됨.
• 테스트가 격리되도록 같은 환경을 초기화하는 용도.

```
beforeEach(() => {
vi.spyOn(fs, 'existsSync').mockImplementation((filePath) => {
return filePath === validImagePath;
});

vi.spyOn(fs, 'readFileSync').mockImplementation((filePath) => {
if (filePath === validImagePath) {
return Buffer.from('mock-image-content');
}	
```
- 변수로 지정한 파일의 위치와 테스트에서의 요청한 위치가 같으면 true 반환

