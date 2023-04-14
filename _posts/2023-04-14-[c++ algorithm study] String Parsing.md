---
layout: post
title: "[c++ 알고리즘 공부] String Parsing"
date: 2023-04-14 19:14:17 +0900
categories: [Algorithm]
tags: [Algorithm, String, Parsing]
---
## LEVEL 24 25

LANGUAGE: C++
OVERVIEW: Parsing

## String

- String이란?
    
    ```cpp
    #include <iostream>
    #include <string>
    // = C++의 string class
    
    #include <cstring>
    // = C의 string.h
    // = #include <string.h>
    // strlen(a),  strcpy(a) ...
    
    // 이제 사용 X
    // C,C++의 문자열 불편 이유
    // 문자열을 char 배열에 넣어야 함
    // 비교가 안돼서 strcmp
    // 복사가 안돼서 strcpy
    // for문 로직 포함되어 있는 streln (효율성X)
    
    int main(){
    	string str="BBQ";
    	str="WER";
    	cout<<str;
    
    	if(str=="WER"){
    		cout<<"같다"
    	}
    
    	string strArray[3]; //string 배열
    	char firstAlphabet=str[1]; //string 특정 원소 
    	int strLen=str.length(); //=str.size() //for문 로직 포함X (효율성O)
    }
    ```
    
- string 역순출력
    
    ```cpp
    //역순 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string str;
    	cin >> str;
    	for (int i = str.length()-1; i >= 0; i--) {
    		cout << str[i];
    	}
    }
    ```
    
    ```cpp
    //string 배열에서 역순 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string bucket[4] = { "ABBT","BTBT","BBBT","KFC" };
    	for (int i = 0; i < 4; i++) {
    		for (int j = bucket[i].length() - 1; j >= 0; j--) {
    			cout << bucket[i][j];
    		}
    		cout << "\n";
    	}
    }
    ```
    
- string 덧셈
    
    ```cpp
    //string 덧셈
    
    string a="ABC";
    string b="DEF";
    string c=a+b+"G"; //ABCDEFG
    b+="DEF" //DEFDEF
    string d="ABC"+"DEF"; //X (연산자 오버로딩으로 작동되기 때문에 string class가 적어도 하나 필요)
    string d=string("ABC")+string("DEF");//ABCDEF
    ```
    
- class 개념 : method 포함 가능
    
    ```cpp
    //C struct : method 내장 불가능
    //C++ struct : method 내장 가능 (=class)
    
    //class : method 내장 가능 (ex. struct, string)
    //string의 method : length(), find(), ...
    struct structEX{
    	int a;
    	int b;
    	void funcEX(){
    		cout<<"#";
    	}
    };
    
    int main(){
    	structEX varEX;
    	varEX.a=1;
    	varEX.b=2;
    	varEX.funcEX();
    }
    ```
    
- String의 method : find()
    
    ```cpp
    //1. string의 method : find()
    
    string stringEX="SBKFCSDFSD"
    int kfcIdx=stringEX.find("KFC");
    int kfcIdx2=stringEX.find("KFC",4); //4번 인덱스부터 찾아라 (default=0)
    cout<< kfcIdx; //2 (없으면 -1)
    ```
    
    ```cpp
    //string 배열에서 BB 가 있으면 O 없으면 X 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string bucket[4] = { "ABBT","BTBT","BBBT","KFC" };
    	for (int i = 0; i < 4; i++) {
    		if (bucket[i].find("BB")!=-1)cout <<"O";
    		else cout << "X";
    	}
    }
    ```
    
- String의 method : substring()
    
    ```cpp
    //2. string의 method : substring()
    // insert, delete, ...
    
    string dummy="ABCSDFW";
    string ret=dummy.substr(2,4);//2번 idx부터 4글자 추출 =CSDF
    string retEnd=dummy.substr(2);//2번 idx부터 끝까지
    ```
    
    ```cpp
    //A~Z 문자열 만들고 a index부터 b index까지 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string str;
    	for (char i = 'A'; i <= 'Z'; i++) {
    		str += i;
    	}
    //char 연결 가능
    
    	int a, b;
    	cin >> a >> b;
    
    	cout << str.substr(a, b - a + 1);
    }
    ```
    
- parsing : 1개 parsing
    
    ```cpp
    //parsing
    //[] 안의 값만 뽑아내기
    //ex. 금융권 회사 기출문제
    ```
    
    ```cpp
    //입력받은 문자열에서 [] 안의 값 parsing
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string str;
    	cin >> str;
    
    	int startIdx = str.find("[");
    	int endIdx = str.find("]",startIdx+1); //startIdx+1 넣어야 효율성 증대
    
    	cout << str.substr(startIdx+1, endIdx - startIdx - 1);
    }
    ```
    
    ```cpp
    //string 배열에서 [] 있는 string만 parsing하여 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string str[4] = { "ABCQ","B[4]R","CCDA","BT[15]" };
    	for (int i = 0; i < 4; i++) {
    		int a = str[i].find("[");
    		int b = str[i].find("]",a+1);
    		if (a == -1)continue;
    		cout << str[i].substr(a + 1, b - a - 1) << "\n";
    	}
    }
    ```
    
- parsing : n개 parsing
    
    ```cpp
    //[] 안의 숫자의 합 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string str = "B[45]AB[9944]";
    	int a = 0, b=0;
    	int sum = 0;
    	while(1){
    		a = str.find("[",b+1);
    		if (a == -1)break;
    		b = str.find("]",a+1);
    		sum += stoi(str.substr(a + 1, b - a - 1));
    	}
    	cout << sum;
    }
    ```
    
    ```cpp
    //ABC 개수 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string str = "ABCSFWABCDJSABCD";
    	int cnt = 0;
    	int a = -1;
    	while (1) {
    		a = str.find("ABC", a + 1);
    		if (a == -1)break;
    		cnt++;
    	}
    	cout << cnt;
    }
    ```
    
    ```cpp
    //string 배열에서 GOLD 개수 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int parsing(string input) {
    	int a = -1;
    	int cnt = 0;
    	while (1) {
    		a = input.find("GOLD", a + 1);
    		if (a == -1)break;
    		cnt++;
    	}
    	return cnt;
    }
    
    int main() {
    	string str[4] = { "GOLDABGOLD","GOLDTTTT","AGOLDGOLD","GOLDTTTT" };
    	int cnt = 0;
    	for (int i = 0; i < 4; i++) {
    		cnt+=parsing(str[i]);
    	}
    	cout << cnt;
    }
    ```
    
    ```cpp
    //string에서 target들의 빈출 정도 출력
    //어떤 기능을 함수로 뺄지 고민하기
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int parsing(string input, string target) {
    	int a = -1, cnt = 0;
    	while (1) {
    		a = input.find(target, a + 1);
    		if (a == -1)break;
    		cnt++;
    	}
    	return cnt;
    }
    
    int main() {
    	string str = "ABCSDKFCABCWABCSSD";
    	string target[3];
    	for (int i = 0; i < 3; i++)cin >> target[i];
    	for (int i = 0; i < 3; i++) {
    		cout<<parsing(str, target[i])<<" ";
    	}
    }
    ```
    
    ```cpp
    //두 문자열 A,B 중 짧은 문자열이 긴 문자열 안에 몇 번 나타나는지 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int parsing(string input, string target) {
    	int a = -1, cnt = 0;
    	while (1) {
    		a = input.find(target, a + 1);
    		if (a == -1)break;
    		cnt++;
    	}
    	return cnt;
    }
    
    int main() {
    	string A, B;
    	cin >> A >> B;
    	if (A.length() > B.length())cout<<parsing(A, B);
    	else cout<<parsing(B, A);
    }
    ```
    
    ```cpp
    //2개의 # 이외의 문자열 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string str = "AB#BTTR#KKKK";
    
    	int a = str.find("#", 0);
    	int b = str.find("#", a + 1);
    
    	cout<<str.substr(0, a)<<" ";
    	cout<<str.substr(a+1, b-a-1)<<" ";
    	cout << str.substr(b + 1, str.length() - b-1);
    }
    ```
    
    ```cpp
    //n개의 [] 사이의 문자열 출력 
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string str = "ABC[BTS]KK[KFC]GGR[TT][11]";
    	int a = -1, b=-1;
    	while (1) {
    		a = str.find("[",a+1);
    		b = str.find("]",b+1);
    		if (a == -1)break;
    		cout<<str.substr(a+1, b-a-1)<<" ";
    	}
    }
    ```
    
    ```cpp
    // | 기준 파싱하여 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string str;
    	cin >> str;  //1942|1995|87
    	str+="|";
    	int a = 0,b=-1;
    	while (1) {
    		b = str.find("|", a);
    		if (b == -1)break;
    		cout << str.substr(a, b-a-1)<<" ";
    		a=b+1;
    	}
    }
    ```
    
    ```cpp
    // split(결과배열, 결과배열인덱스, 문자열, 기준이 될 문자열)
    // string을 기준으로 잘라서 vect 배열에 넣는 함수 구현    
    
    void split(string result[], int& rn, string str, string tar) {
    	int a = 0, b = 0;
    	str += tar;
    	while (1) {
    		b = str.find(tar, a);
    		if (b == -1)break;
    		result[rn++]=str.substr(a, b - a);
    		a = b + 1;
    	}
    }
    
    int main() {
    	string str = "ID|SDFAS|DFS|DD";
    	int rn = 0;
    	string result[10];
    	split(result, rn, str, "|");
    	for (int i = 0; i < 10; i++) {
    		if (result[i] == "")break;
    		cout << result[i] << " ";
    	}
    }
    ```
    
    ```cpp
    // - 사이에 있는 수들의 합 출력
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string str = "12-3342-1-3-12";
    	str += "-";
    	int a = 0, b = -1, sum = 0;
    	while (1) {
    		b = str.find("-", a);
    		if (b == -1)break;
    		sum+=stoi(str.substr(a, b - a));
    		a = b + 1;
    	}
    	cout << sum;
    }
    ```
    
    ```cpp
    // : , 사이의 숫자 파싱 후 합 구하기
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int main() {
    	string str = "ID:48,BTS:56,HHHTT:99";
    	str += ",";
    	int a = -1, b = -1,sum=0;
    	while (1) {
    		a = str.find(":",a+1);
    		b = str.find(",",b+1);
    		if (a == -1)break;
    		sum += stoi(str.substr(a + 1, b - a - 1));
    	}
    	cout << sum;
    }
    ```
    
- C 문자열 ↔ C++ string
    
    ```cpp
    //C 문자열 -> C++ string
    char buf[10]="ASD";
    string str=buf; // = string(buf)
    
    //C++ string -> C 문자열
    str.c_str();
    strcpy(buf, str.c_str());
    ```
    
- string ↔ int
    
    ```cpp
    string str="2342";
    
    int i=stoi(str); //string "234" -> integer 234
    string s=to_string(i); //integer 234 -> string "234"
    ```
    
- String의 method : erase(), insert() → replace구현
    
    ```cpp
    string str = "ABCEFDG";
    str.erase(3,2) //3번 인덱스에 2글자를 지움
    str.insert(1,"<HI>") //1번 인덱스에 <HI> 글자를 넣음
    ```
    
    ```cpp
    // replace 함수 구현 (=문자열 대체 함수)
    
    // 실습 : KFC 를 ***로 대체하기
    
    string str = "ABCKFC__KFC";
    int a = 0, b = 0;
    while (1) {
    	b = str.find("KFC", a);
    	if (b == -1)break;
    	str.erase(b, 3);
    	str.insert(b, "***");
    	a = b + 1;
    }
    ```
    
- 유효성 검사
    
    ```cpp
    유효성 검사 (Valid Check)
    허용되는 문자열 검사 (후 파싱)
    ex. 비밀번호 유효한지 검사
    
    함수로 뺴고 조건마다 return 시키기
    
    AB<<<14>><>><<123> (X)
    AB<132>BT<142> (O)
    ```
    
    ```cpp
    // 유효성 검사 실습
    
    // 2~5글자 허용
    // 특수문자 1개까지 허용
    // 대문자 불가
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    bool isValid(string str) {
    	int len = str.length();
    	int cnt = 0;
    	// 2~5글자 허용
    	if (len < 2 || len > 5)return false;
    	// 특수문자 1개까지 허용
    	// 대문자 불가
    	for (int i = 0; i < len; i++) {
    		if (str[i] >= '0' && str[i] <= '9')cnt = cnt;
    		else if( str[i]>='a'&&str[i] <= 'z') cnt = cnt;
    		else if (str[i]>='A'&&str[i] <= 'Z')return false;
    		else cnt++;
    		if (cnt == 2)return false;
    	}
    	return true;
    }
    
    int main() {
    	string str;
    	cin >> str;
    	cout << isValid(str);
    }
    ```
    
    ```cpp
    // 유효성 검사 실습 = 네이버 신입사원 기출문제
    
    //이름@도메인.탑레벨도메인
    //이름 : 영어 소문자 & .
    //도메인 : 영어 소문자
    //탑레벨도메인 : com, net, org
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    int solution(string str[4], int strLen) {
    
    }
    
    int main() {
    	string str[4];
    	for (int i = 0; i < 4; i++) {
    
    	}
    }
    ```