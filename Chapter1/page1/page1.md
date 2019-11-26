# windows窗口遍历
```
#include <iostream>
#include <Windows.h>

using namespace std;

BOOL CALLBACK EnumChildProc(_In_ HWND hwnd, _In_ LPARAM lParam)
{
	char szTitle[MAX_PATH] = { 0 };
	char szClass[MAX_PATH] = { 0 };
	int nMaxCount = MAX_PATH;

	LPSTR lpClassName = szClass;
	LPSTR lpWindowName = szTitle;

	GetWindowTextA(hwnd, lpWindowName, nMaxCount);
	GetClassNameA(hwnd, lpClassName, nMaxCount);
	cout << "[Child window] window handle: " << hwnd << " window name: "
		<< lpWindowName << " class name " << lpClassName << endl;

	return TRUE;
}


BOOL CALLBACK EnumWindowsProc(HWND hwnd, LPARAM lParam)
{

	char szTitle[MAX_PATH] = { 0 };
	char szClass[MAX_PATH] = { 0 };
	int nMaxCount = MAX_PATH;

	LPSTR lpClassName = szClass;
	LPSTR lpWindowName = szTitle;

	GetWindowTextA(hwnd, lpWindowName, nMaxCount);
	GetClassNameA(hwnd, lpClassName, nMaxCount);
	cout << "[Parent window] window handle: " << hwnd << " window name: "
		<< lpWindowName << " class name " << lpClassName << endl;

	EnumChildWindows(hwnd, EnumChildProc, lParam);

	return TRUE;
}


int main(int argc, char *argv[])
{
	EnumWindows(EnumWindowsProc, 0);
	return 0;
}

```


 


