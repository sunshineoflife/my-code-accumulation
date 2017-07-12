windows下批量修改文件后缀，.c-->.cpp

```c++
bool isCFile(CString strFile)
{
	char strSrc[512];
	strcpy(strSrc, strFile.GetBuffer(0));
	strFile.ReleaseBuffer();
	char drive[_MAX_DRIVE];
	char dir[_MAX_DIR];
	char ext[_MAX_EXT];
	char fname[_MAX_FNAME];
	_splitpath(strSrc, drive, dir, fname, ext);
	if (strcmp(ext, ".c") == 0)
	{
		return true;
	}

	return false;
}

void cvtC2CPP(CString strFile)
{
	char strSrc[512];
	strcpy(strSrc, strFile.GetBuffer(0));
	strFile.ReleaseBuffer();
	char drive[_MAX_DRIVE];
	char dir[_MAX_DIR];
	char ext[_MAX_EXT];
	char fname[_MAX_FNAME];
	_splitpath(strSrc, drive, dir, fname, ext);
	char strDes[512]; strDes[0] = '\0';
	sprintf_s(strDes, "%s.cpp", fname);

	char strCmdLine[512];
	sprintf_s(strCmdLine, "ren %s %s", strSrc, strDes);//ren命令，strDes是文件名称，不带路径
	system(strCmdLine);
}


void BrowseCurrentAllFile(CString strDir)
{
	if(strDir == _T(""))
	{
		return;
	}
	else
	{
		if(strDir.Right(1) != _T("//"))
			strDir += L"//";
		strDir =strDir+_T("*.*");
	}
	CFileFind finder;
	CString strPath;
	BOOL bWorking = finder.FindFile(strDir);
	while(bWorking)
	{
		bWorking = finder.FindNextFile();
		strPath = finder.GetFilePath();
 		if(finder.IsDirectory() && !finder.IsDots())
		{
			BrowseCurrentAllFile(strPath); //递归调用
		}
		else if(!finder.IsDirectory() && !finder.IsDots())
		{
			//strPath就是所要获取的文件路径
			printf("%s\n", strPath);
			if (isCFile(strPath))
			{
				cvtC2CPP(strPath);
			}
		}

	}
}


void CrenameC2CPPDlg::OnBnClickedBtnCvt()
{
	BrowseCurrentAllFile(_T("F:\\mame-libretro\\mame2010\\mame2010-libretro\\src"));
}
```
