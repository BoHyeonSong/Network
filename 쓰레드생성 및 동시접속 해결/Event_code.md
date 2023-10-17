```
#pragma comment (lib,"ws2_32.lib")
#include <WinSock2.h>
#include <WS2tcpip.h>

#include <iostream>
#include <process.h>

using std::cout;
using std::endl;

int const NumOfThread = 4;
HANDLE hevent;
HANDLE th[4];

void CreateEventandThread();
unsigned __stdcall ThreadProc(LPVOID);
int main()
{

	cout << "main thread id : " << GetCurrentThreadId() << " start" << endl;
	//Create Event & Thread : Event(manual rest, non-signalled)
	//thread 4. arg NULL
	CreateEventandThread();

	//2 second wait
	Sleep(2000);

	//Event non-sig -> sig
	SetEvent(hevent);

	//Wait untill thread is finised
	WaitForMultipleObjects(NumOfThread, th, true, INFINITE);
	cout << "main thread id : " << GetCurrentThreadId() << " end" << endl;

	return 0;
}

void CreateEventandThread()
{
	//CreateEvent(manual reset, non-sig
	hevent = CreateEvent(NULL, true, false, NULL);

	//CreateThread
	for (int i = 0; i < NumOfThread; i++)
	{
		th[i] = (HANDLE)_beginthreadex(NULL, 0, &ThreadProc, NULL, 0, NULL);
	}
}

unsigned __stdcall ThreadProc(LPVOID)
{
	cout << "New Thread : " << GetCurrentThreadId() << endl;
	cout << "Wait until Event Object become signalled" << endl;
	WaitForSingleObject(hevent, INFINITE);
	cout << "Thread : " << GetCurrentThreadId() << " will be finished" << endl;
	return 0;
}
```
