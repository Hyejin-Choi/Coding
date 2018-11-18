//공학도를 위한 공학용계산기 프로그램
#include<stdio.h>
#include<math.h>
#include<string.h>
#include<windows.h>
#define Number_ 26
#define operator_ 20
#define PI 3.14159265
void    Finalcaculation(char*);        // 연산식 최종
double  calculation(char*);         // 연산식 계산하기
void    input_operator(char*);      // 대입 연산자 계산
void    arithmeticorder(char*);     // 사칙연산 우선순위정하기
double  funcArit(int, double);  // 수학 함수 호출
double  parenthesis(char*, int*);   // 괄호순서 정하기
double  Value__[Number_];      // 전역변수로 저장
char    Value_[Number_];       // 전역변수
char    *mathfunction[operator_] = {"ACOS", "ASIN", "ATAN", "COS", "SIN", "TAN","COSH", "SINH", "TANH", "ACOSH", "ASINH", "ATANH","EXP", "LOG", "LOG10","SQRT", "FABS", "FLOOR", };                         
 double deg2rad(double degree)
{
	return degree*PI/180;     //삼각함수 각도-> 라디안 change
}
int operator___(char *str, char A)
{
	int i;
	for (i = 0; str[i] != '\0'; ++i)
	
	if (str[i] == A)return 1;
 	return 0;
}
 int Search(char *str, char *place)
{
	int i, j, flag = 0;
	for (i = 0; str[i] != '\0'; ++i) {
		for (j = 0; place[j] != '\0' && place[j] == str[i + j]; ++j);
		if (place[j] == '\0') return i;
	}
 	return -1;
}
double atod(char *str)              //아스키로되어있는실수문자->double로
{
	int a = 1;
	double number = 0;
 	while (*str != '\0') {
		if (*str <= '9' && *str >= '0')
			if (a == 1) number = number * 10 + *str - '0';
			else {
				number += (double)(*str - '0') / a;
				a *= 10;
			}
		else if (*str== '.') a *= 10;
		++str;
	}
	return number;
}
 int main() {
	char input_number[127];
	int i, j, act;
	char oo;
 	printf(" \t\t\t\t\t\t:: 계산기 프로그램  :: \n");
	printf("■■■■■■■■■■■■■■■■■■■■■■■■■ ::Introduce Operator ::■■■■■■■■■■■■■■■■■■■■■■\n");
	printf(" :: 계산을 시작하려면 Enter를 입력해주세요\n");
	printf(" :: 덧셈연산 '+'\n ");
	printf(":: 뺄셈연산 '-'\n ");
	printf(":: 곱셈연산 '*'\n ");
	printf(":: 나눗셈연산 '/'\n");
	printf(" :: sin함수 'sin'\n");
	printf(" :: cos함수 'cos'\n");
	printf(" :: tan함수 'tan'\n");
	printf(" :: asin함수 'asin'\n");
	printf(" :: acos함수 'acos'\n");
	printf(" :: atan함수 'atan'\n");
	printf(" :: sinh함수 'sinh'\n");
	printf(" :: cosh함수 'sonh'\n");
	printf(" :: tanh함수 'tanh'\n");
	printf(" :: acosh함수'acosh'\n");
	printf(" :: asinh함수'asinh'\n");
	printf(" :: atanh함수'atanh'\n");
	printf(" :: exp연산  'exp'\n");
	printf(" :: log연산  'log'\n");
	printf(" :: log10연산'log10'\n");
	printf(" :: sqrt연산 'sqrt'\n");
	printf(" :: fabs연산 'fabs'\n");
	printf(" :: floor연산 'floor'\n");
	printf(" ::계산을 마칠려면 end를 입력해주세요\n");
	printf("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■\n");
 	printf("계산을 시작하시려면 Enter를 입력해주세요!");
	scanf("%c", &oo);
	fflush(stdin);
	if (oo == '\n') {
		system("cls");
 	}
 	printf("\n■■■■■■■■■■■■■■■■■■■■■■■■::계산기 작동중::■■■■■■■■■■■■■■■■■■■■■■■■■■■\n");
	for (i = 0; i < Number_; ++i) {
		Value_[i] = 0;
 	}
 	while (1) {
		
printf(":: 연산하고 싶은 식을 입력하고 Enter를 눌러주세요:-) (끝내고 싶으면 end를 입력해주세요) \n");
		act = 0;
		while (1) {
input_number[act] = getchar();              // 연산식 입력
	if (input_number[act] == '\n') {
	input_number[act] = '\0';
	break;
}                             
	++act;
		}
 for (i = 0; input_number[i] != '\0'; ++i)
while (input_number[i] == ' ')
for (act = i; input_number[act] != '\0'; ++act)
input_number[act] = input_number[act + 1];          // 띄어쓰기없애기
 		
for (i = 0; input_number[i] != '\0'; ++i)
if (input_number[i] <= 'z' && input_number[i] >= 'a')
input_number[i] += 'A' - 'a';                        // 소문자->대문자
 // end 명령일 경우 프로그램을 종료
if (!strcmp(input_number, "END")) {
printf("■■■■■■■■■■■■■■■■■■■■■■■■■■ :: 계산종료 ::■■■■■■■■■■■■■■■■■■■■■■■■■■\n");
			break;
		}
Finalcaculation(input_number);
	}
}
 void Finalcaculation(char *command)
{
	int i, a;
	double result;
 	result = calculation(command);  //계산하기
 	if (!(result <= 0 || result >= 0))
	printf("= 오류오류오류오류\n"); //계산값이 이상하면 오류출력
	else 
	printf("= %f\n", result);//계산값이 정상이면 계산값출력
 }
 double parenthesis(char *arr, int *i)   //괄호!!
{
	int a = 0, b = 1;         //b는 닫혀있지 않은 괄호 갯수! ->0이되면 calculation함수로 넘어가서 계산
	char input_number[127];
	double score;
 	while (b) {
		++(*i);
		if (arr[*i] == '\0') return 0;
		if (arr[*i] == '(') ++b;
		if (arr[*i] == ')') --b;
		input_number[a++] = arr[*i];
	}
	input_number[--a] = '\0';
 	score = calculation(input_number);
 	if (!(score <= 0 || score >= 0)) 
		return 0;
	else 
		return score;
}
 double calculation(char *str)
{
	int i, j;
	int x = 0, o = 0, cb;
	double arr1[100], score;
	char operator[100], arrr[20], ar, fixoperator = 'f';
 	for (i = 0; str[i] != '\0'; ++i) {        //괄호경우!  parenthesis함수 넘긴 후에 계산
		
		if (str[i] == '(') {
			score = parenthesis(str, &i);
			if (!(score <= 0 || score >= 0)) return 0;
			arr1[x++] = score;
			if (fixoperator == '-') arr1[x- 1] = -arr1[x - 1];
		}
 		
else if (str[i] <= '9' && str[i] >= '0' || str[i] == '.') {  // 식에서 그냥 숫자만 나오면 그냥 arr1 배열에 집어넣는다.
	cb = 0;
	while (str[i] <= '9' && str[i] >= '0' || str[i] == '.')
	arrr[cb++] = str[i++];
	arrr[cb] = '\0';
	arr1[x++] = atod(arrr);
	if (fixoperator == '-') arr1[x - 1] = -arr1[x - 1];
	--i;
		}
 		
else if (operator___("*/+-", str[i])) {               //// 연산자가 나오면 operator배열에 넣는다.
			if (o == x) {
				if (operator___("*/", str[i]) || fixoperator != 'd') return 0;
				else fixoperator = str[i];
			}
			else {
				fixoperator = 'd';
				operator[o++] = str[i];
			}
		}
 		// 영문자 (수학함수) 
		else if (str[i] <= 'Z' && str[i] >= 'A') {
                   for (j = 0; j < operator_; ++j)             //수학함수 수학함수 계산후에 배열arr에 넣기
	if (Search(str + i, mathfunction[j]) == 0 && str[strlen(mathfunction[j]) + i] == '(') {
		i += strlen(mathfunction[j]);
		score = parenthesis(str, &i);
		if (!(score <= 0 || score >= 0)) return 0;
		arr1[x++] = funcArit(j, score);
		if (fixoperator == '-') arr1[x - 1] = -arr1[x - 1];
		break;
					}
			
		}
	}
 	if (o + 1 != x) return 0;  //수식정상 ->operator 배열의 길이가 1더큼.
 	
							   
	// 계산된 결과를 arr1배열의 i+1에 저장한다
	for (i = 0; i < o; ++i) {
	if (operator___("+-", operator[i]))   // operator배열에서 *나 /를 찾아 계산하기
			continue;
 		if (operator[i] == '*') arr1[i + 1] = arr1[i] * arr1[i + 1];            //계산과정 !
		if (operator[i] == '/') arr1[i + 1] = arr1[i] / arr1[i + 1];             //arr1배열에 저장
 		operator[i] = 'd';           //계산후 d 저장! ->계산을 끝냈다는 것을 알림
	}
 	
	for (i = 0; i < o; ++i) {
		if (operator[i] == 'd')                 
			continue;
 for (j = i + 1; operator[j] == 'd' && j < o; ++j);
 if (operator[i] == '+') arr1[j] = arr1[i] + arr1[j];       //operator배열에서 +나 -를 계산하기  
if (operator[i] == '-') arr1[j] = arr1[i] - arr1[j];      //arr1배열에 저장
	}
 	
	return arr1[o];                // ar11배열의 마지막 원소를 리턴  ->계산결과
}
 void input_operator(char *orderr)
{
	int i, j, cnt = 0;
	double result[20];
	
if (orderr[0] <= 'Z' && orderr[0] >= 'A' && orderr[1] == '=') {  //Calculation 에서 계산 한 후 Value에 저장하기
result[0] = calculation(orderr + 2);
		if (result[0] <= 0 || result[0] >= 0) {
		Value_[orderr[0] - 'A'] = 1;
		Value__[orderr[0] - 'A'] = result[0];
		return;
		}
		else {
			printf("= 오류오류오류오류\n");
			return;
		}
	}
 	
	printf("= 오류오류오류오류\n");
	return;
}
 void arithmeticorder(char *orderr)
{
 	int i;
	int co = 0, ca = 0;
	char o[100], a[100];
 	for (i = 0; orderr[i] != '\0'; ++i)
		if (orderr[i] <= 'Z' && orderr[i] >= 'A') {
			a[ca++] = orderr[i] - 'A';
			++i;
		}
		else if (operator___("+-*/", orderr[i]))                //사칙연산 !
			o[co++] = orderr[i];
 }
 double funcArit(int Function, double x)
{
	// 수학 라이브러리 함수
 	switch (Function) {
	case 0: return acos(deg2rad(x));
	case 1: return asin(deg2rad(x));
	case 2: return atan(deg2rad(x));
 	case 3: return cos(deg2rad(x));
	case 4: return sin(deg2rad(x));
	case 5: return tan(deg2rad(x));
 	case 6: return cosh(deg2rad(x));
	case 7: return sinh(deg2rad(x));
	case 8: return tanh(deg2rad(x));
 	case 9 : return acosh(deg2rad(x));
	case 10 : return asinh(deg2rad(x));
	case 11 : return atanh(deg2rad(x));
 	case 12: return exp(x);
	case 13: return log(x);
	case 14: return log10(x);
 	case 15: return sqrt(x);
	case 16: return fabs(x);
	case 17: return floor(x);
	default: return 0;
	}
}
