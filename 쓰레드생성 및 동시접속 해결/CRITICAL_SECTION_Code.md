```
#pragma comment (lib,"ws2_32.lib")
#include <WinSock2.h>
#include <WS2tcpip.h>

#include <iostream>
#include<process.h>

using std::cout;
using std::endl;

unsigned __stdcall f(LPVOID);

CRITICAL_SECTION cs; // application 레벨의 동기화 위한 키


int a;
int main()
{
	InitializeCriticalSection(&cs);

	SYSTEM_INFO sys;
	GetSystemInfo(&sys);
	cout << "Number of core : " << sys.dwNumberOfProcessors << endl;
	HANDLE th[10];
	for (int i = 0; i < sys.dwNumberOfProcessors; i++) {
		th[i] = (HANDLE)_beginthreadex(NULL, 0, &f, NULL, 0, NULL);
		//WaitForSingleObject(th[i], 1000); 
		
	}
	WaitForMultipleObjects(sys.dwNumberOfProcessors, th, true, INFINITE);

	
	Sleep(1000);
	cout << "main end a : " << a << endl;
	return 0;
}

unsigned __stdcall f(LPVOID)
{
	//cout << __FUNCTION__ << endl;
	for (int i = 0; i < 100000; i++) {
		EnterCriticalSection(&cs);
		a++;
		LeaveCriticalSection(&cs);
	}
	return 0;
}
```
