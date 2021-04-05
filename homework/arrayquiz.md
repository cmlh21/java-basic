배열 추가문제

하-1 : int형 6칸 짜리 배열을 생성하세요.

```java
package arrayquiz.low;

 class Quiz01 {
	public static void main(String[] args) {
		int[] arr = new int[6];
	}
}
```



하-2 : 다음 출력 결과를 예상하세요.
```
int[] a = new int[2]; 
System.out.println(a[0]); // 답 : 0
System.out.println(a[1]); // 답 : 0
			  // int형 배열을 만들면 자동으로 초기값 0이 들어가기 때문

double[] b = new double[2];
System.out.println(b[0]); // 답 : 0.0
System.out.println(b[1]); // 답 : 0.0
			  // double형 배열을 만들면 자동으로 초기값 0.0이 들어가기 때문

String[] c = new String[2];
System.out.println(c[0]); // 답 : null 
System.out.println(c[1]); // 답 : null
			  // String형 배열을 만들면 자동으로 초기값 null이 들어가기 때문

char[] d = new char[2];
System.out.println(d[0]); // 답 : ??? 
System.out.println(d[1]); // 답 : ??? 
			  // char형 배열을 만들면 자동으로 초기값 \0이 들어가기 때문

boolean[] e = new boolean[2];
System.out.println(e[0]); // 답 : false
System.out.println(e[1]); // 답 : false
			  // boolean형 배열을 만들면 자동으로 초기값 false가 들어가기 때문
```


하-3 : 사용자에게 배열의 칸 개수를 입력 받고, 해당 정수의 크기만큼 정수형 배열을 생성하세요.
	입력 : 3  ===> 결과 : [ 0, 0, 0 ] 
	입력 : 5  ===> 결과 : [ 0, 0, 0, 0, 0 ]

```java
package arrayquiz.low;

import java.util.Scanner;

public class Quiz03 {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.print("칸 개수 입력 : ");
		int[] arr = new int[sc.nextInt()];

		for (int i = 0; i < arr.length; ++i) {
			System.out.print(arr[i] + "  ");
		}
	}
}
```



하-4 : (3)번에서 생성된 배열에 다음 기능을 추가하세요.
	ㄱ. 0 ~ n-1 까지의 숫자를 배열에 순서대로 저장하세요.
		입력 : 3  ===> 결과 : [0, 1, 2]
		입력 : 5  ===> 결과 : [0, 1, 2, 3, 4]
	ㄴ. 배열의 가장 마지막 원소부터 0번 원소까지 역순으로 출력하세요.
	ㄷ. for문을 사용하여 배열에 저장된 실제 원소들을 역순으로 재배치 하세요. (난이도 중)
	    sysout(배열[0]); // 3
	    sysout(배열[1]); // 2
	    sysout(배열[2]); // 1
	    sysout(배열[3]); // 0
	   (for문을 쓰되 for문의 실행 횟수가 n/2이 되도록하세요. 
         (5칸 배열이면 2회만에, 10칸 배열이면 5회만에 for문이 종료되도록)

```java
package arrayquiz.low;

import java.util.Scanner;

public class Quiz04 {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.print("칸 개수 입력 : ");
		int[] arr = new int[sc.nextInt()];

		// ㄱ. 0 ~ n-1 까지의 숫자를 배열에 순서대로 저장하세요.
		for (int i = 0; i <= arr.length - 1; ++i) {
			System.out.print((arr[i] = i) + "  ");
		}
		System.out.println();

		// ㄴ. 배열의 가장 마지막 원소부터 0번 원소까지 역순으로 출력하세요.
		for (int i = arr.length - 1; i >= 0; --i) {
			System.out.print((arr[i] = i) + "  ");
		}
		System.out.println();

		// ㄷ. for문을 사용하여 배열에 저장된 실제 원소들을 역순으로 재배치 하세요.
		for (int i = 0; i < arr.length / 2; ++i) {
			System.out.print((arr[i] = arr[arr.length - 1 - i]) + "  ");
		}
		if (arr.length / 2 == 1)
			System.out.print((arr[arr.length / 2] = arr[arr.length / 2]) + "  ");
		for (int i = arr.length-1; i >= arr.length / 2; --i) {
			System.out.print((arr[i] = arr[arr.length - 1 - i]) + "  ");
		}

	}
}
```



하-5 : 사용자에게 배열의 칸 개수를 입력 받고, 해당 정수의 크기만큼 String형 배열을 생성하고
	입력 : 3  ===> 결과 : [ null, null, null ] 
	입력 : 5  ===> 결과 : [ null, null, null, null, null ]
	사용자에게 입력받은 단어들을 순차적으로 배열에 저장하세요.  

```java
package arrayquiz.low;

import java.util.Scanner;

public class Quiz05 {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.print("칸 개수 입력 : ");
		int num = sc.nextInt();
		String[] arr = new String[num];

		// 사용자에게 입력받고 저장
		for (int i = 0; i <= arr.length -1; ++i) {
			System.out.print((i + 1) + "번째 단어 입력 : ");
			arr[i] = sc.next();
		}
		
		// 배열 출력
		for (int i = 0; i <= arr.length -1; ++i) {
			System.out.print(arr[i] + "  ");
		}
	}
}
```



중-2 : dates 배열은 다음과 같이 1~12월의 최대 일자가 들어있습니다. 
	 ==> int[] dates = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}; 

	1) dates 배열을 활용하여 1/1일부터 사용자에게 입력 받은 월/일 까지 며칠이 소요되는지 출력하세요.
	   단, 사용자에게 해는 따로 입력받지 않기때문에 윤년은 없다고 가정합니다.
	
		예) 월 : 12   일 : 31  ==> 364일 소요!
		    월 : 4    일 : 12   ==> 101일
		원리) 4/12 의 결과를 계산하려면
		    1 / 1 ~ 1 / 31  => 31 (dates[0]) 
		    2 / 1 ~ 2 / 28  => 28 (dates[1])
		    3 / 1 ~ 3 / 31  => 31 (dates[2])
	    	4 / 1 ~ 4 / 12  => 12 (사용자가 입력한 일)
		 => 31 + 28 + 31 + 12 - 1 = 101일 
	
	2) 시작월/일과 목표 월/일 을 각각 입력 받고 d-day 계산기를 만드세요.
	   단, year는 입력받지 않기때문에 d-day의 최댓값은 364일로 가정합니다.
	   예) 시작 : 9/26  목표 : 11/23  ==> 결과 : d-day 58 !!!
	       시작 : 1/1 목표 : 12/31  ==> 결과 : d-day 364 !!!
	       시작 : 12/31 목표 : 1/1  ==> 결과 : d-day 1 !!!
	       시작 : 4/12 목표 : 4/11  ==> 결과 : d-day 364 !!!
	   원리)
		start : 1/1 ~ 시작 월/일까지 며칠이 소요되는지 계산합니다. 
		end : 1/1 ~ 목표 월/일까지 며칠이 소요되는지 계산합니다. 
		end-start 를 합니다. 
		이때 음수가 나온다면 목표일이 시작일보다 앞서있다는 의미입니다. (즉 목표일이 이듬해)
		이 경우 +365를 하면 됩니다.

```java
package arrayquiz.mid;

import java.util.Scanner;

public class Quiz02 {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int[] dates = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
		
		// 1)
		System.out.print("월 입력 : ");
		int month = sc.nextInt();
		System.out.print("일 입력 : ");
		int day = sc.nextInt();
		int sum = 0;
		for (int i = 0; i < month - 1; ++i) {
			sum += dates[i];
		}
		System.out.println(sum + day - 1 + "일");
		System.out.println();
		
		// 2)
		System.out.print("시작 날짜 입력 (월, 일) : ");
		int startm = sc.nextInt();
		int startd = sc.nextInt();
		System.out.print("목표 날짜 입력 (월, 일) : ");
		int endm = sc.nextInt();
		int endd = sc.nextInt();
		
		int startSum = 0; // 시작 날짜 총 일수 세기
		for (int i = 0; i < startm - 1; ++i) {
			startSum += dates[i];
		}
		int startdate = startSum + startd - 1;
		
		int endSum = 0; // 목표 날짜 총 일수 세기
		for (int i = 0; i < endm - 1; ++i) {
			endSum += dates[i];
		}
		int enddate = endSum + endd - 1;
		
		int dday = 0; // 목표날짜 - 시작날짜
		if (startdate > enddate)
			dday = enddate + 365 - startdate;
		else dday = enddate - startdate;
		System.out.println("d-day " + dday + " !!!");

	}
}
```



중-3: "김", "이", "박", "최", "한" 등의 대한민국 성씨를 저장할 배열을 만들고, 성씨들을 저장하세요.
       "피카츄", "라이츄", "파이리", "꼬부기", "버터풀", "야도란", "피죤투" 를 저장할 배열을 만들고 이름들을 저장하세요.
       1) 총 10개의 성+이름 조합을 출력하세요. ( Math.random()을 사용하며, 중복 조합을 허용합니다)
       2) 조합가능한 모든 이름을 출력하세요.

```java
package arrayquiz.mid;

public class Quiz03 {
	public static void main(String[] args) {
		String[] lastName = { "김", "이", "박", "최", "한", "하", "조", "옹", "윤", "배", "정", "강" };
		String[] firstName = { "피카츄", "라이츄", "파이리", "꼬부기", "버터풀", "야도란", "피죤투" };

		// 1) 총 10개의 성+이름 조합 출력
		for (int i = 0; i < 10; ++i) {
			int r1 = (int) (Math.random() * lastName.length - 1);
			int r2 = (int) (Math.random() * firstName.length - 1);
			System.out.println(lastName[r1] + firstName[r2]);
		}
		System.out.println("--------");

		// 2) 조합가능한 모든 이름을 출력하세요.
		// 입력
		String[][] name = new String[lastName.length][firstName.length];
		for (int i = 0; i < lastName.length; ++i) {
			for (int j = 0; j < firstName.length; ++j) {
				name[i][j] = lastName[i] + firstName[j];
			}
		}
		
		// 출력
		for (int i = 0; i < lastName.length; ++i) {
			for (int j = 0; j < firstName.length; ++j) {
				System.out.println(name[i][j]);
			}
		}
	}
}
```



