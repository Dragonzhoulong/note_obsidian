# ls. c
## Fmtname
``` c
char*

fmtname(char *path)

{

  static char buf[DIRSIZ+1];

  char *p;

  

  // Find first character after last slash.

  for(p=path+strlen(path); p >= path && *p != '/'; p--)

    ;

  p++;

  

  // Return blank-padded name.

  if(strlen(p) >= DIRSIZ)

    return p;

  memmove(buf, p, strlen(p));

  memset(buf+strlen(p), ' ', DIRSIZ-strlen(p));

  return buf;

}
```

``` c
char* 
fmtname(char* path){
	static char buf[DIRSZIE+1];
	char* p;
	//找到路径中最后一个/后面的字符
	for(p=path+strlen(path),p>=path&&*p!='/';p--)
	;
	p++;
	if(strlen(p)>=DIRSIZ)
		return p;
	memove(buf,p,strlen(p));
	memset(buf+strlen(p),' ',DIRZIE-strlen(p));
	return buf;
}
```

## 关于 strlen () 函数
strlen 如同名字所示是返回字符串的长度 (仅仅是长度例如"fuck"返回的是 4, 所以从起始地址+4=> `\0`)
`sizeof` 函数返回的是数据结构的长度所以是包含了字符串结尾默认添加的 `\0`

```c
void

ls(char *path)

{

  char buf[512], *p;

  int fd;

  struct dirent de;

  struct stat st;

  

  if((fd = open(path, 0)) < 0){

    fprintf(2, "ls: cannot open %s\n", path);

    return;

  }

  

  if(fstat(fd, &st) < 0){

    fprintf(2, "ls: cannot stat %s\n", path);

    close(fd);

    return;

  }

  

  switch(st.type){

  case T_FILE:

    printf("%s %d %d %l\n", fmtname(path), st.type, st.ino, st.size);

    break;

  

  case T_DIR:

    if(strlen(path) + 1 + DIRSIZ + 1 > sizeof buf){

      printf("ls: path too long\n");

      break;

    }

    strcpy(buf, path);

    p = buf+strlen(buf);

    *p++ = '/';

    while(read(fd, &de, sizeof(de)) == sizeof(de)){

      if(de.inum == 0)

        continue;

      memmove(p, de.name, DIRSIZ);

      p[DIRSIZ] = 0;

      if(stat(buf, &st) < 0){

        printf("ls: cannot stat %s\n", buf);

        continue;

      }

      printf("%s %d %d %d\n", fmtname(buf), st.type, st.ino, st.size);

    }

    break;

  }

  close(fd);

}
```

``` c
void
ls(char* path){
	char buf[512], *p;
	int fd;
	struct dirent de;
	struct stat st;
	if((fd=open(path,0))<0){
		fprintf(2,"ls:cannot open %s\n",path);
		return;
	}
	if(stat(fd,&st)<0){
		fprintf(2,"ls: cannot stat %s\n",path);
		close(fd);
		return;
	}
	switch(st.type){
		case T_FILE:
			printf("%s %d %d %l\n",fmtname(path),st.type,st.ino,st.size);
		case T_DIR:
			if(strlen(path)+1+DIRSIZ+1>sizeof(buf)){
				fprintf(2,"ls: path too long \n");
				break;
			}
			strcpy(buf,path);
			p=buf+strlen(buf);
			*p++ = '/';
			while(read(fd,&de,sizeof(de))==sizeof(de)){
				if(de.inum==0)
			}
	
	}
	
	
	
}
```

# fd 
file descriptor
``` c

fd = open (pathname, flags, mode);

rlen = read (fd, buf, count);

wlen = write(fd,buf,count);

status = close(fd);

```

## Inode 是个啥?

inode 包含文件的元信息

+ 文件的字节数
+ 文件拥有者的 User ID
+ 文件的 Group ID
+ 文件的读 , 写, 执行权限
+ 文件的时间戳
