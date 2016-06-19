# Achive
Test a artile's words
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#define size 100000
#define length 10000
void main()
{
	char a[size],temp[20],c;
	int b[size];//array b means the condition of array a
	int i,n,l,p,q,x,z;//'l' means 'lengh','n' means 'number'
	struct Vocabulary
	{  char v[20];
	   int num;}lex[length];//words list
	struct Vocabulary tempo;
	FILE *fp,*FP;
    char input[20],output[20];
    
    for(i=0;i<=length;i++)
		lex[i].num=0;//initialize the struct array

	printf("Please input the article's address:");
	gets(input);
	printf("Please input the vocabulary list's address:");
    gets(output);

	if((fp=fopen(input,"r+"))==NULL)
	{    printf("cannot open the article file");
	      exit(0);
	}
	if((FP=fopen(output,"rb"))==NULL)
	{	printf("cannot open the words list file");
	exit(0);}//open the file

    for(i=0;!feof(fp);i++)
	      a[i]=fgetc(fp);
	a[i]='\0';
	fclose(fp);//convert data from file into this program

    for(i=0;i<length;i++)
	    fread(&lex[i],sizeof(struct Vocabulary),1,FP);//convert words list form file into this program
	fclose(FP);
    
	printf("Display the words list or not?(y/n)");
    scanf("%c",&c);
	if(c=='y')
    {printf("Now,display the list:\n");
    for(i=0;i<length;i++)
	{   if(lex[i].num!=0)
		printf("%20s%5d\n",lex[i].v,lex[i].num);
        else if(lex[i].num==0)
			{z=i;printf("This list had %d words \n",z);break;}
	}
	}
	   else 
		   printf("\n");
	//display the former list

	l=strlen(a)-1;
	for(i=0;i<=l;i++)
		if(a[i]>='A'&&a[i]<='Z')
			a[i]=a[i]+'a'-'A';//lower
	if(a[0]>='a'&&a[0]<='z'){
		for(i=l;i>=0;i--)
		   a[i+1]=a[i];
	    a[0]=' ';//put a blank at the start
		l++;
	    a[l+1]='\0';}
    if(a[l]>='a'&&a[l]<='z')
    {
		l++;
		a[l]='.';
		a[l+1]='\0';
	}
    for(i=0;i<=l;i++)
		if(a[i]>='a'&&a[i]<='z')
			b[i]=1;//1 means  the current character is a alphabet
		else
			b[i]=0;//0 means  it is a something else		
	for(i=0,n=0;i<=l;i++)
	   if(b[i]==1 && b[i+1]==0)
			n++;
	printf("This article has %d words\n",n);

	for(i=0;i<=l;i++)
		if(b[i]==0&&b[i+1]==1)
		{    p=i+1;//varible 'p' is the head of a word's location
		     for(q=p;;q++)
			    if(b[q]==1&&b[q+1]==0)
		                break;//varible 'q' is the location of an end of a word
			 for(x=0;p<=q;p++,x++)
			    temp[x]=a[p];//convert the word in array 'a' to a temporary array
			 temp[x]='\0';
			 for(n=0;n<=length;n++)
			    if(strcmp(temp,lex[n].v)==0)
				{  lex[n].num++;break;}
             if(n==length+1)
			 {strcpy(lex[z].v,temp);
		     lex[z].num=1;
			 z++;}
		}
	z--;//'z'is the current number of words in the list

   for(i=0;i<=z-1;i++)
   {	 for(x=i,n=i+1;n<=z;n++)
            if(lex[x].num<lex[n].num)
				x=n;
		 tempo=lex[i];lex[i]=lex[x];lex[x]=tempo;
   }
   //sort

   for(i=0;i<=length;i++)
   {	if(lex[i].num==0)
	       break;
        printf("The frequency of %s is %d\n",lex[i].v,lex[i].num);}
   printf("This article has %d different words\n",i);//ending

   printf("Store or not?(y/n)\n");
   scanf(" %c",&c);//add a space beforehand
   if(c=='y'){
   		if((FP=fopen(output,"wb"))==NULL)
		{	printf("cannot open the words list file");
		exit(0);}//open the file
	    for(i=0;i<=length;i++)
		 {	if(lex[i].num==0)
	        break;
	        if(fwrite(&lex[i],sizeof(struct Vocabulary),1,FP)!=1)
            printf("file write error");}
        fclose(FP);
        printf("Having stored\n");
   }
   else if(c=='n')
	   printf("End\n");
}
