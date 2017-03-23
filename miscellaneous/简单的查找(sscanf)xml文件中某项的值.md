- # xml文件   
```xml
<game>
    <vibriorMode>0</vibriorMode>
    <Config>1</Config>
    <enableAudio>0</enableAudio>
</game>

```

- # 读取Config的值    
```c
int getConfigNum(const char* strGameConfigFile)
{
    FILE *fp;
    char strLine[512] = "";
    int nGameConfigNum = 1;
    
    if((fp = fopen(strGameConfigFile,"r")) == NULL)//game_config.xml
    {
        return nGameConfigNum;
    }
    while (!feof(fp))
    {
        fgets(strLine,512,fp);
        //"    <Config>%d</Config>\n"
        char str1[20] = "";
        char str2[20] = "";
        char str3[20] = "";
        int ret = sscanf(strLine, "%[^<]<%[^>]>%[^<]", str1, str2, str3);
        if (ret==3 && strcmp(str2, "Config")==0) {
            nGameConfigNum = atoi(str3);
            break;
        }
    }
    fclose(fp);
    return nGameConfigNum;
}
```
- # 说明
%[^<]表示读到<为止，后面<读走紧接着的<，后面同理。   
实际应用要麻烦很多，要注意str1、str2、str3空间是否足够，要判断返回值读到几个有效字串。   
