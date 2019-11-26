# 指定进程遍历窗口
```
#include <windows.h>  
#include <TlHelp32.h>  
#include <atlstr.h>  
#include <locale.h>  
#include <tchar.h>
#define WINDOW_TEXT_LENGTH 256  

BOOL CALLBACK EnumChildWindowCallBack(HWND hWnd, LPARAM lParam)
{
	DWORD dwPid = 0;
	GetWindowThreadProcessId(hWnd, &dwPid); // 获得找到窗口所属的进程  
	if (dwPid == lParam) // 判断是否是目标进程的窗口  
	{
		printf("0x%08X    \n", hWnd); // 输出窗口信息  
		TCHAR buf[WINDOW_TEXT_LENGTH];
		SendMessage(hWnd, WM_GETTEXT, WINDOW_TEXT_LENGTH, (LPARAM)buf);
		wprintf(L"%s\n", buf);
		EnumChildWindows(hWnd, EnumChildWindowCallBack, lParam);    // 递归查找子窗口  
	}
	return TRUE;
}

BOOL CALLBACK EnumWindowCallBack(HWND hWnd, LPARAM lParam)
{
	DWORD dwPid = 0;
	GetWindowThreadProcessId(hWnd, &dwPid); // 获得找到窗口所属的进程  
	if (dwPid == lParam) // 判断是否是目标进程的窗口  
	{
		printf("0x%08X    \n", hWnd); // 输出窗口信息  
		TCHAR buf[WINDOW_TEXT_LENGTH];
		SendMessage(hWnd, WM_GETTEXT, WINDOW_TEXT_LENGTH, (LPARAM)buf);
		wprintf(L"%s\n", buf);
		EnumChildWindows(hWnd, EnumChildWindowCallBack, lParam);    // 继续查找子窗口  
	}
	return TRUE;
}

int main(int argc, WCHAR** argv)
{
	setlocale(LC_CTYPE, "chs");
	//printf("%s\n", argv[1]);
	DWORD Pid = 0;    // 进程id  
	PROCESSENTRY32 pe;  // 进程信息  
	pe.dwSize = sizeof(PROCESSENTRY32);
	HANDLE hSnapshot = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0); // 进程快照  
	if (!Process32First(hSnapshot, &pe)) // 得到第一个进程的快照  
		return 0;
	do
	{
		if (!Process32Next(hSnapshot, &pe))
			return 0;
	} while (StrCmp(pe.szExeFile, _T("MFCApplication1.exe")));  // 遍历进程直到找打目标进程  
	Pid = pe.th32ProcessID;
	wprintf(L"exe process: 0x%08X\n", Pid);
	EnumWindows(EnumWindowCallBack, Pid);
	return 0;
}

```
![avatar](https://github.com/haidragon/window_Banalysis/blob/master/Chapter2/page2/images/2.png)


