# Network

```
#include<iostream>
#include<winsock2.h>

using std::cout;
using std::endl;

void err_display(const char*);

int main() {
	

	err_display("Si bal error");

	return 0;
}

void err_display(const char* mes) {
	LPVOID lpbuffer = nullptr;// void * lpbuffer

	FormatMessage(FORMAT_MESSAGE_ALLOCATE_BUFFER | FORMAT_MESSAGE_FROM_SYSTEM,
		nullptr, 10054, MAKELANGID(LANG_ENGLISH, SUBLANG_DEFAULT),
		(LPSTR)&lpbuffer, 0, nullptr);

	cout << mes << " : " << (char*)lpbuffer << endl;
	LocalFree(lpbuffer);
}
```
