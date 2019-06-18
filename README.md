# Show-Box
It is a school project which is like an imdb site this lets you bookmark or view what shows are trending or are top rated. But first movies and TV shows database has to be entered
/*
------------------------------------------------------------------------------
|============================================================================|
||==========================================================================||
|||==================== WELCOME TO SHOW BOX ===============================|||
||==========================================================================||
|============================================================================|
------------------------------------------------------------------------------*/
/**********************************************************
* MADE BY: RISHABH AGRAWAL & VAIBHAV SAHNI                *
* CLASS: XII                                              *
* SECTION: A                                              *
* SESSION: 2017-2018                                   	  *
* SCHOOL: RYAN INTERNATIONAL SCHOOL, MAYUR VIHAR          *
************************************************************/
//=============================================================================
//HEADER FILE USED
//=============================================================================
 #include<dos.h>
 #include<fstream.h>
 #include<process.h>
 #include<conio.h>
 #include<string.h>
 #include<stdio.h>
 #include<ctype.h>
 #include<limits.h>
 //=============================================================================
 //FUNCTIONS USED
 //=============================================================================
 void Udatabase();
 int  inputchoice();
 int  Newuser();
 int  Search();
 int  TSearch(int);
 void Euser();
 int  PersonalI(int);
 void Movies(int);
 void Series(int);
 void TModify(int,int);
 void TDelete(int,int);
 void TAdd(int);
 void Show_Bookmarks(int,int);
 void Movies(int);
 int  Browse(int,int);
 void GBrowse();
 void Allshow(int);
 int  CheckName(char []);
 void BookmarkDelete(int,int,int);
 void shift(int,int [],int);
 void BDeleteU(int,int);
//=============================================================================
//STRUCTURE FOR TV
//=============================================================================
 struct TV
 {
	char Tname[60];
	int  Tcode;
	int  Tcategory[3];
 };
//=============================================================================
//CLASS OF USER
//=============================================================================
 class User
 {
	char Uname[20];
	char Uemail[40];
	char UserID[40];
   public:
	int SBookmark[20];
	int MBookmark[20];
	int SNOB;

	int MNOB;
	void Enter();                //Function For Entering the Data of User.
	void Show()                  //Function For Showing the Data of User.
	{
		 cout<<"\nNAME:       ";
		 puts(Uname);
		 cout<<"USERNAME:    "<<UserID;
		 cout<<"\nEMAIL:     "<<Uemail<<endl;
		 getch();
	}
	char *ReturnUser()           //Function For Returning the User ID of User.
	{
		 return UserID;
	}
	int  Write();                //Function For Writing the Data of User.
	void Read();                 //Function For Readinng the Data of User.
	void Modify();               //Function For Modifing the Data of User.
	int  Delete(int);            //Function For Deleting the Data of User.
	void Add_Bookmark(int, int, int );	//Function For Bookmarking .
	User()
	{
		 strcpy(Uname,"Null");
		 strcpy(UserID,"Null");
		 strcpy(Uemail,"Null");
		 SNOB=0;
		 MNOB=0;
	}
	~User()
	{}
 };
//=============================================================================
//CLASS FOR AUTOGENERATING CODE
//=============================================================================
 class tv_config
 {
  public:
	int Code_Movies;
	int Code_Shows;
	tv_config()
	{
			Code_Movies=1;
			Code_Shows=1;

	}
	~tv_config()
	{

	}
 };
//=============================================================================
//FUCTION FOR CHECKING NAME
//=============================================================================
 int CheckName( char name[20])
 {
	int flag=0;
	for(int i=0;i<strlen(name);i++)
	{
		if((name[i]!=' ') && !(isalpha(name[i])))
			flag=1;
	}
	return flag;


 }
//=============================================================================
//FUCTION FOR ENTERING USER'S DATA
//=============================================================================
 void User::Enter()
	{
		 lb:
		 cout<<"\nENTER YOUR NAME ";
		 gets(Uname);
		 if(CheckName(Uname)==1)
		 {
			cout<<"\nINVALID NAME PLEASE TRY AGAIN ";
			goto lb;
		 }
		 cout<<"\nENTER YOUR USERNAME ";
		 cin>>UserID;
		 cout<<"\nENTER EMAIL ";
		 cin>>Uemail;
	}
//=============================================================================
//FUNCTION FOR WRITING THE DATA OF USER ONTO THE FILE
//=============================================================================
 int User::Write()
 {
	User C,u;
	ifstream fin;
	int flag=0;
	char ans;
	fin.open("User.dat",ios::binary|ios::beg);
	lb:
	u.Enter();
	while(fin.read((char*)&C,sizeof(C)))
	{
		if (strcmpi(C.ReturnUser(),u.ReturnUser())==0)
		{
			cout<<"\nENTERED USER ID ALREADY EXISTS";
			cout<<"\nDO YOU WANT TO TRY AGAIN(T) OR OPEN AN EXISTING ID(E)";
			cin>>ans;
			if(ans=='T'||ans=='t')
				goto lb;
			else
			{
				flag=1;
				return flag;
			}
		}
	}
	fin.close();
	if(flag!=1)
	{
		u.MNOB=0;
		u.SNOB=0;
		cout<<"\nID Has Been Created ";
		ofstream fout;
		fout.open("User.dat",ios::binary|ios::ate);
		fout.write((char*)&u,sizeof(u));
		fout.close();
	}
	return -1;
 }
//=============================================================================
//FUNCTION FOR READING THE DATA OF USER FROM THE FILE
//=============================================================================
 void User::Read()
 {
	User C;
	ifstream fin("User.dat",ios::binary);
	cout<<"\nData is: ";
	while(fin.read((char*)&C,sizeof(C)))
	{
		C.Show();
	}
	fin.close();

 }
//=============================================================================
//FUCTION FOR MODIFYING THE DATA OF THE USER
//=============================================================================
 void User::Modify()
  {
	lb:
	clrscr();
	char ch='n';
	int flag=1;
	char str[40];
	ifstream fin;
	fin.open("User.dat",ios::binary);
	Show();
	cout <<"\nDO YOU WANT TO MODIFY THE DATA " <<endl;
	cin>>ch;
	if(ch=='y'||ch=='Y')
	{
		do
		{
			cout<<"\nENTER NEW NAME (To retain old value ente '.') ";
			gets(str);
			if(strcmp(str,".")==0)
			break;
		}while(CheckName(str));
		if(strcmp(str,".")!=0)
			strcpy(Uname,str);
		do
		{
		      cout<<"ENTER NEW USERNAME (To retain old value ente '.') "<<endl;
		      cin>>str;
		      if(strcmp(ReturnUser(),str)==0)
		      {
				flag=0;
				cout<<"This Username Already Exists Please Enter Again "<<endl;
		      }
		      else
				flag=1;
		}while(flag==0);
		if(strcmp(str,".")!=0)
		       strcpy(UserID,str);
		cout<<"ENTER NEW EMAIL (To retain old value ente '.') ";
		cin>>str;
		if(strcmp(str,".")!=0)
		       strcpy(Uemail,str);
	}
	if ((ch!='y' && ch!='Y') && (ch!='n' && ch!='N'))
	{
		cout<<"\nWRONG INPUT ENTER AGAIN ";
		getch();
		goto lb;
	}
	cout<<"\nDATA MODIFIED ";
	getch();

 }
//=============================================================================
//FUNCTION FOR DELETING ID
//=============================================================================
 int User::Delete(int pos)
 {
	 User C;
	 int loc,flag=0;
	 ifstream fin;
	 fin.open("User.dat",ios::binary|ios::beg);
	 ofstream fout;
	 fout.open("Temp.dat",ios::binary);
	 while(fin.read((char*)&C,sizeof(C)))
	 {
		loc=fin.tellg()-(sizeof(C));
		if (pos==loc)
		{
			char ans;
			C.Show();
			lb:
			cout<<"\nARE YOU SURE YOU WANT TO DELETE YOUR ID (Y/N): "<<endl;
			cin>>ans;

			if (ans=='n')
				fout.write((char*)&C,sizeof(C));
			else if (ans=='y')
				flag=1;
			else
			{
				cout<<"\nINVALID ANSWER TRY AGAIN ";
				goto lb;
			}
		}
		else
			fout.write((char*)&C,sizeof(C));
		}
		fin.close();
		fout.close();
		remove("User.dat");
		rename("Temp.dat","User.dat");
		if (flag==1)
		{
			cout<<"\n Data has been deleted ";
			getch();
			return 4;
		}
		return 1;

 }
//=============================================================================
//FUNCTION FOR CREATING NEW ID
//=============================================================================
 int Newuser()
 {
	 clrscr();
	 User C;
	 int flag;
	 cout<<"================================================================================";
	 cout<<"******************************CREATING NEW ID***********************************";
	 cout<<"================================================================================";
	 flag=C.Write();
	 getch();
	 if(flag==1);
	 {
		return 2;
	 }

 }
//=============================================================================
//FUNCTION FOR SEARCING USER'S ID
//=============================================================================

 int Search()
 {
	 int pos;
	 ifstream fin;
	 fin.open("User.dat",ios::binary);
	 if(!fin)
	 {
		 cout<<"\n File not found";
		 exit(-1);
	 }
	 User u1;
	 char uname[40];
	 int flag=0;
	 cout<<"\n ENTER YOUR USER ID: ";
	 gets(uname);
	 while(fin.read((char*)&u1,sizeof(u1)))
	 {
		 if(strcmpi(u1.ReturnUser(),uname)==0)
		 {
			flag=1;
			pos=fin.tellg()-(sizeof(u1));
			return pos;
		}
	 }
	 fin.close();
	 cout<<"\n";
	 if( flag==0)
		return -1;

	 return -1;
	}
//=============================================================================
//FUNCTION FOR PERSONAL ID MENU
//=============================================================================

 int PersonalI(int pos)
 {
	 User U;
	 int ch;
	 fstream fio;
	 fio.open("User.dat",ios::binary|ios::out|ios::in);
	 do
	 {
		 clrscr();
		 fio.seekg(pos);
		 fio.read((char*)&U,sizeof(U));
		 cout<<"================================================================================";
		 cout<<"*******************************PERSONAL INFO************************************";
		 cout<<"================================================================================";
		 cout<<"\n1) VIEW DATA ";
		 cout<<"\n2) EDIT DATA ";
		 cout<<"\n3) DELETE YOUR ID ";
		 cout<<"\n4) BACK "<<endl;
		 ch=inputchoice();
		 switch(ch)
		 {
			case 1:cout<<"\nLOADING...............";
			       delay(500);
			       cout<<"\n YOUR ID ";
			       U.Show();
			       break;
			case 2:cout<<"\nLOADING...............";
			       delay(500);
			       U.Modify();
			       fio.seekp(pos);
			       fio.write((char*)&U,sizeof(U));
			       //cout<<"\nData modified";
			       break;
			case 3:cout<<"\nLOADING...............";
			       delay(500);
			       cout<<"\nDELETING YOUR ID";
			       ch=U.Delete(pos);
			       if(ch==4)
					return ch;
			       break;
			case 4:cout<<"\nBack ";
			       break;
			default :cout<<"\nWRONG CHOICE";
		};

	}while(ch!=4);
	fio.close();
	return 0;
 }
//=============================================================================
//FUNCTION OF EXISTING USER
//=============================================================================

 void Euser()
 {
	clrscr();
	int pos,ch;
	char uid[40];
	pos=Search();
	if(pos==-1)
	{
		cout<<"\The USERNAME DOES NOT EXIST";
		getch();
	}
	else
	{
		do
		{
			clrscr();
			cout<<"================================================================================";
			cout<<"*******************************WELCOME BACK*************************************";
			cout<<"================================================================================";
			cout<<"\n1) PERSONAL INFO" ;
			cout<<"\n2) MOVIES ";
			cout<<"\n3) TV SHOWS";
			cout<<"\n4) BACK"<<endl;
			ch=inputchoice();;
			switch(ch)
			{
			case 1 :cout<<"\nLOADING...............";
				delay(500);
				ch=PersonalI(pos);
				break;
			case 2 :cout<<"\nLOADING...............";
				delay(500);
				Movies(pos);
				break;
			case 3 :cout<<"\nLOADING...............";
				delay(500);
				Series(pos);
				break;
			case 4 :break;
			default :cout<<"\nWRONG CHOICE";
				 getch();
		};
	}while(ch!=4);
	}
 }
//=============================================================================
//FUNCTION FOR ADDING TV SHOWS OR MOVIES
//=============================================================================

 void Tadd(int a)
 {
	ifstream fin;
	fin.open("Tconfig.dat",ios::binary|ios::beg);
	TV T;
	tv_config c;
	fin.read((char*)&c,sizeof(c));
	fin.close();
	char y;
	ofstream fout;
	if(a==2)
	{
		fout.open("Shows.dat",ios::binary|ios::ate);
		c.Code_Shows++;
		T.Tcode=c.Code_Shows;
	}
	else if(a==1)
	{
		fout.open("Movies.dat",ios::binary|ios::ate);
		c.Code_Movies++;
		T.Tcode=c.Code_Movies;
	}
	cout<<"================================================================================";
	cout<<"********************************ENTERING DATA***********************************";
	cout<<"================================================================================";
	cout<<"\nNAME ";
	gets(T.Tname);
	cout<<"DO YOU WANT TO ADD IT IN THE CATEGORY UPCOMING" ;
	cin>>y;
	if(y=='y')
		T.Tcategory[0]=1;
	else
		T.Tcategory[0]=0;
	cout<<"DO YOU WANT TO ADD IT IN THE CATEGORY TRENDING " ;
	cin>>y;
	if(y=='y')
		T.Tcategory[1]=1;
	else
		T.Tcategory[1]=0;
	cout<<"DO YOU WANT TO ADD IT IN THE CATEGORY ALL TIME BEST " ;
	cin>>y;
	if(y=='y')
		T.Tcategory[2]=1;
	else
		T.Tcategory[2]=0;
	fout.write((char*)&T,sizeof(T));
	fout.close();
	fout.open("Tconfig.dat",ios::binary|ios::trunc|ios::ate);
	fout.write((char*)&c,sizeof(c));
	fout.close();
 }
//==============================================================================
//FUNCTION FOR SEARCHING MOVIE OR TV SHOW
//=============================================================================

 int TSearch(int a)
 {
	int pos;

	ifstream fin;
	if(a==2)
	fin.open("Shows.dat",ios::binary|ios::beg);
	else if(a==1)
	fin.open("Movies.dat",ios::binary);
	if(!fin)
	{
		cout<<"\n File not found";
		exit(-1);
	}
	TV T;
	int flag=0,code;
	cout<<"\nENTER MOVIE/SERIES CODE ";
	code=inputchoice();
	while(fin.read((char*)&T,sizeof(T)))
	{
		if(T.Tcode==code)
		{
			flag=1;
			pos=fin.tellg()-(sizeof(T));
			return pos;
		}
	}
	fin.close();
	cout<<"\n";
	if( flag==0)
		return -1;
	return -1;
	}
//=============================================================================
//FUNCTION FOR MODIFYING TV'S DATA
//=============================================================================

 void TModify(int pos,int a)
 {
	TV T;
	char ch='n',y;
	fstream fio;
	if(a==2)
		fio.open("Shows.dat",ios::binary|ios::in|ios::out|ios::ate);
	else if(a==1)
		fio.open("Movies.dat",ios::binary|ios::in|ios::out|ios::ate);
	fio.seekg(pos);
	fio.read((char*)&T,sizeof(T));
	cout<<"================================================================================";
	cout<<"***********************************MODIFY***************************************";
	cout<<"================================================================================";
	cout<<"\nNAME "<<T.Tname<<"\nCODE "<<T.Tcode;
	for(int i=0;i<3;i++)
	cout<<"\nCATEGORY "<<i+1<<" "<<T.Tcategory[i];
	lb:
	cout << "\nDO YOU WANT TO MODIFY ";
	cin>>ch;
	if (ch=='y'||ch=='Y')
	{
		cout<<"\nMODIFYING THE DATA ";
		cout<<"\nNEW NAME ";
		cin>>T.Tname;
		cout<<"DO YOU WANT TO ADD IT IN THE CATEGORY UPCOMING " ;
		cin>>y;
		if(y=='y')
			T.Tcategory[0]=1;
		else
			T.Tcategory[0]=0;
		cout<<"DO YOU WANT TO ADD IT IN THE CATEGORY TRENDING " ;
		cin>>y;
		if(y=='y')
			T.Tcategory[1]=1;
		else
			T.Tcategory[1]=0;
		cout<<"DO YOU WANT TO ADD IT IN THE CATEGORY ALL TIME BEST " ;
		cin>>y;
		if(y=='y')
			T.Tcategory[2]=1;
		else
			T.Tcategory[2]=0;
		fio.seekp(pos);
		fio.write((char*)&T,sizeof(T));
		fio.close();
		cout<<"\nDATA MODIFIED ";
	}
	if ((ch!='y' && ch!='Y') && (ch!='n' && ch!='N'))
	{
		cout<<"\nWRONG INPUT ENTER AGAIN ";
		getch();
		goto lb;
	}

 }
//=============================================================================
//FUNCTION FOR ACCESSING TV'S DATABASE
//=============================================================================

 void TDelete(int pos,int a)
 {
	TV T;
	int loc,flag=0,code;
	ifstream fin;
	if(a==2)
		fin.open("Shows.dat",ios::binary|ios::beg);
	else if(a==1)
		fin.open("Movies.dat",ios::binary|ios::beg);
	ofstream fout;
	fout.open("Temp.dat",ios::binary);
	cout<<"================================================================================";
	cout<<"**********************************DELETE****************************************";
	cout<<"================================================================================";
	while(fin.read((char*)&T,sizeof(T)))
	{
		loc=fin.tellg()-(sizeof(T));
		if (pos==loc)
		{code=T.Tcode;
			char ans;
			cout<<"\nNAME"<<T.Tname<<"\nCODE "<<T.Tcode;
			cout<<"\nENTER (y) IF YOU WANT TO DELETE THE DATA : "<<endl;
			cin>>ans;
			if(ans=='y'||ans=='Y')
				flag=1;
			else
				fout.write((char*)&T,sizeof(T));
		}
		else
			fout.write((char*)&T,sizeof(T));
	}
	fin.close();
	fout.close();
	if (flag==1)
	{
		cout<<"\n DATA HAS BEEN DELETED ";
		BDeleteU(code,(a-1));
	}
	if(a==2)
	{
		remove("Shows.dat");
		rename("temp.dat","Shows.dat");
	}
	if(a==1)
	{
		remove("Movies.dat");
		rename("Temp.dat","Movies.dat");
	}

 }

//=============================================================================
//FUNCTION FOR ALTERING TV'S DATABASE
//=============================================================================

 void Talter()
 {
	TV T;
	int ch,a;
	int pos;
	do
	{
		lb:
		clrscr();
		cout<<"\n\nENTER THE DATABASE YOU WANT TO CHANGE ";
		cout<<"\n1)MOVIES";
		cout<<"\n2)SERIES\n";
		a=inputchoice();
		if(a!=1 && a!=2)
		{
			cout<<"\nWRONG INPUT PLEASE ENTER AGAIN ";
			getch();
			goto lb;
		}
		cout<<"================================================================================";
		cout<<"****************************WELCOME TO DATABASE*********************************";
		cout<<"================================================================================";
		cout<<"\n1) ADD NEW DATA" ;
		cout<<"\n2) MODIFY DATA ";
		cout<<"\n3) DELETE DATA ";
		cout<<"\n4) SEE ALL DATA ";
		cout<<"\n5) BACK"<<endl;
		ch=inputchoice();;
		switch(ch)
		{
			case 1:cout<<"\nLOADING...............";
			       delay(500);
			       clrscr();
			       Tadd(a);
			       break;
			case 2:cout<<"\nLOADING...............";
			       delay(500);
			       clrscr();
			       pos=TSearch(a);
			       if(pos== -1)
			       {
					cout<<"\nNO DATA WAS FOUND ";
					ch=5;
					cout<<"\nGOING BACK TO MAIN MENU ";
					getch();
			       }
			       else
			       {
					clrscr();
					TModify(pos,a);
			       }
			       break;
			case 3:cout<<"\nLOADING...............";
			       delay(500);
			       clrscr();
			       pos=TSearch(a);
			       if(pos== -1)
			       {
					cout<<"\nNO DATA WAS FOUND ";
					ch=5;
					cout<<"\nGOING BACK TO MAIN MENU ";
					getch();
			       }
			       else
			       {
					clrscr();
					TDelete(pos,a);
			       }
			       break;
			case 4:cout<<"\nLOADING...............";
			       delay(500);
			       clrscr();
			       Allshow(a);
			       break;
			case 5:break;
			default :cout<<"\nWRONG CHOICE";
		};
	}while(ch!=5);
 }
//=============================================================================
//FUNCTION FOR MOVIES
//=============================================================================

 void Movies(int pos)
 {
	int ch;
	User u1;
	do
	{
		clrscr();
		cout<<"================================================================================";
		cout<<"****************************NOW SHOWING MOVIES**********************************";
		cout<<"================================================================================";
		cout<<"\n1)BROWSE MOVIES ";
		cout<<"\n2)YOUR BOOKMARKS ";
		cout<<"\n3)BACK "<<endl;
		ch=inputchoice();;
		switch (ch)
		{
			case 1:cout<<"\nLOADING...............";
			       delay(500);
			       Browse(1,pos);
			       break;
			case 2:cout<<"\nLOADING...............";
			       delay(500);
			       Show_Bookmarks(1,pos);
			       break;
			case 3:break;
			default:cout<<"Invalid choice";
		};
	}while(ch!=3);
 }
//=============================================================================
//FUNCTION FOR SERIES
//=============================================================================

 void Series(int pos)
 {
	int ch;
	User u1;
	do
	{
		clrscr();
		cout<<"================================================================================";
		cout<<"****************************NOW SHOWING SERIES**********************************";
		cout<<"================================================================================";
		cout<<"\n1)BROWSE SERIES ";
		cout<<"\n2)YOUR BOOKMARKS ";
		cout<<"\n3)BACK "<<endl;
		ch=inputchoice();;
		switch (ch)
		{
			case 1:cout<<"\nLOADING...............";
			       delay(500);
			       Browse(0,pos);
			       break;
			case 2:cout<<"\nLOADING...............";
			       delay(500);
			       Show_Bookmarks(0,pos);
			       break;
			case 3:break;
			default:cout<<"Invalid choice";
		};
	}while(ch!=3);
 }
//=============================================================================
//FUNCTION FOR BROWSE
//=============================================================================

 int Browse(int tv,int pos)
 {
	int ch,code,i,j,m;
	TV T;
	User u;
	char ans;
	lb:
	i=1;j=0;
	clrscr();
	cout<<"================================================================================";
	cout<<"**********************************BROWSE****************************************";
	cout<<"================================================================================";
	cout<<"\n\nSELECT A CATEGORY ";
	cout<<"\n1)UPCOMING ";
	cout<<"\n2)TRENDING ";
	cout<<"\n3)ALL TIME BEST ";
	cout<<"\n4)BACK "<<endl;
	ch=inputchoice();;
	if(ch==4)
		return 0;
	ifstream fin;
	if (tv==1)
		fin.open("Movies.dat",ios::binary|ios::beg);
	else if (tv==0)
		fin.open("Shows.dat",ios::binary|ios::beg);
	cout<<"Code\tName ";
	while (fin.read((char*)&T,sizeof(T)))
	{
		if (T.Tcategory[ch-1]==1)
		{
			cout<<"\n"<<(i++)<<"\t"<<T.Tname;
		}
	}
	if (tv==1)
		cout<<"\nENTER THE CODE OF MOVIE YOU WANT TO BOOKMARK (0 IF YOU DON'T WANT TO) ";
	else if (tv==0)
		cout<<"\nENTER THE CODE OF SERIES YOU WANT TO BOOKMARK (0 IF YOU DON'T WANT TO) ";
	code=inputchoice();
	fin.close();
	if(code!=0)
	{
		if (tv==1)
			fin.open("Movies.dat",ios::binary|ios::beg);
		else if (tv==0)
			fin.open("Shows.dat",ios::binary|ios::beg);
		while (fin.read((char*)&T,sizeof(T)))
		{
		      if (T.Tcategory[ch-1]==1)
		       {
				j++;
				if(j==(code))
					m=T.Tcode;
		       }
		}
		u.Add_Bookmark(m,pos,tv);
	}
	else if(code>=i||code<0)
		cout<<"Invalid choice";
	getch();
	fin.close();
 }
//=============================================================================
//FUNCTION FOR ADDING BOOKMARK
//=============================================================================

 void User::Add_Bookmark(int code,int pos,int tv)
 {
	User u1;
	int flag=1;
	fstream fio;
	fio.open("User.dat",ios::binary|ios::in|ios::out|ios::ate);
	fio.seekg(pos);
	fio.read((char*)&u1,sizeof(u1));
	if (tv==0)
	{
		for(int i=0;i<u1.SNOB;i++)
			if(u1.SBookmark[i]==code)
				flag=0;
		if(flag==0)
			cout<<"\nALREADY BOOKMARKED:D ";
		else
		{
			u1.SBookmark[u1.SNOB]=code;
			u1.SNOB++;
			cout<<"\nBOOKMARK ADDED ";
			getch();
		}
	}
	else if (tv==1)
	{
		for(int i=0;i<u1.MNOB;i++)
			if(u1.MBookmark[i]==code)
				flag=0;
		if(flag==0)
			cout<<"\nALREADY BOOKMARKED:D ";
		else
		{
			u1.MBookmark[u1.MNOB]=code;
			u1.MNOB++;
			cout<<"\nBOOKMARK ADDED ";
			getch();
		}

	}
	fio.seekp(pos);
	fio.write((char*)&u1,sizeof(u1));
	fio.close();
 }
//=============================================================================
//FUNCTION FOR SHOWING BOOKMARKS
//=============================================================================

 void Show_Bookmarks(int tv,int pos)
{
	int NOB,j=1,code;
	char ans;
	User u1;
	TV T;
	ifstream fin,f;
	fin.open("User.dat",ios::binary);
	fin.seekg(pos);
	fin.read((char*)&u1,sizeof(u1));
	if (tv==0)
	{
		NOB=u1.SNOB;
		f.open("Shows.dat",ios::binary);

	}
	else if (tv==1)
	{
		NOB=u1.MNOB;
		f.open("Movies.dat",ios::binary);
	}
	if(NOB==0)
	{
		cout<<"\nNO BOOKMARKS ADDED ";
		//getch();
		goto lb2;
	}
	while (f.read((char*)&T,sizeof(T)))
	{
		for (int i = 0; i<=NOB ; i++)
		{
			if (tv==0)
			{
				if(T.Tcode==u1.SBookmark[i])
					cout<<"\n"<<(j++)<<")\t"<<T.Tname;

			}
			else if (tv==1)
			{
				if(T.Tcode==u1.MBookmark[i])
					cout<<"\n"<<(j++)<<")\t"<<T.Tname;
			}

		}
	}
	f.close();
	fin.close();
	getch();
	if(NOB>0)
	{
		lb:
		cout<<"\nPRESS (y) IF YOU WANT TO DELETE A BOOKMARK ";
		cin>>ans;
		if(ans=='y'||ans=='Y')
		{
			cout<<"\nENTER THE CODE OF THE BOOKMARK YOU WANT TO DELETE ";
			code=inputchoice();
			if(code>0 && code<j)
			{
				if (tv==0)
				{
					BookmarkDelete(u1.SBookmark[code-1],pos,1);

				}
				else if (tv==1)
				{
					BookmarkDelete(u1.MBookmark[code-1],pos,0);
				}
			}
			else
				cout<<"Invalid choice";
		}
		if ((ans!='y' && ans!='Y') && (ans!='n' && ans!='N'))
		{
			cout<<"\nWRONG INPUT ENTER AGAIN ";
			getch();
			goto lb;
		}

	}
	lb2:
	getch();
 }
//=============================================================================
//FUNCTION FOR DELETING BOOKMARKS
//=============================================================================

 void BookmarkDelete(int code,int userpos,int tv)
 {
	User u;
	ifstream fin;
	fin.open("User.dat",ios::binary);
	fin.seekg(userpos);
	fin.read((char*)&u,sizeof(u));
	fin.close();
	if(tv==0)
	{
		for(int i=0;i<=u.MNOB;i++)
			if(u.MBookmark[i]==code)
				shift(i,u.MBookmark,u.MNOB);
		u.MNOB--;
		cout<<"\nBOOKMARK HAS BEEN DELETED ";
		getch();
	}
	else if(tv==1)
	{
		for(int i=0;i<=u.SNOB;i++)
			if(u.SBookmark[i]==code)
				shift(i,u.SBookmark,u.SNOB);
		u.SNOB--;
		cout<<"\nBOOKMARK HAS BEEN DELETED ";
		getch();
	}
	ofstream fout;
	fout.open("User.dat",ios::binary);
	fout.seekp(userpos);
	fout.write((char*)&u,sizeof(u));
	fout.close();
 }
//=============================================================================
//FUNCTION FOR SHIFTING BOOKMARKS
//=============================================================================

 void shift(int index,int arr[],int size)
{
  for(int j=index;j<size;j++)
  arr[j]=arr[j+1];
}

//=============================================================================
//FUNCTION FOR DELETING BOOKMARKS FROM USER
//=============================================================================

 void BDeleteU(int code,int tv )
 {
	int pos;
	User u;
	ifstream fin;
	fin.open("User.dat",ios::binary);
	while (fin.read((char*)&u,sizeof(u)))
	{
		 pos=(fin.tellg())-(sizeof(u));
		 BookmarkDelete(code,pos,tv);
	}
	fin.close();
 }
//=============================================================================
//FUNCTION FOR GUEST BROWSE
//=============================================================================

 void GBrowse()
 {
	int ch,code,i=1;
	TV T;
	char ans,tv;
	ifstream fin;
	do
	{
		clrscr();
		cout<<"\nDO YOU WANT TO SEE MOVIES OR SERIES (m/s): ";
		cin>>tv;
		lb2:
		clrscr();
		cout<<"================================================================================";
		cout<<"*******************************WELCOME GUEST************************************";
		cout<<"================================================================================";
		cout<<"\n\nSELECT A CATEGORY ";
		cout<<"\n1)UPCOMING ";
		cout<<"\n2)TRENDING ";
		cout<<"\n3)ALL TIME BEST ";
		cout<<"\n4)BACK "<<endl;
		ch=inputchoice();;
		if(ch<1 || ch>4)
			goto lb2;
		if (tv=='m')
		{
			fin.open("Movies.dat",ios::binary|ios::beg);
		}
		else if (tv=='s')
		{
			fin.open("Shows.dat",ios::binary|ios::beg);
		}
		if(ch!=4)
		{
			cout<<"\nNAME\tCODE";
		}
		while (fin.read((char*)&T,sizeof(T)))
		{
			if (T.Tcategory[ch-1]==1)
			{
				cout<<"\n"<<(i++)<<"\t"<<T.Tname;
			}
		}
		getch();
	}while(ch!=4);
	fin.close();
 }
//=============================================================================
//FUNCTION FOR SHOWING ALL DATA
//=============================================================================

 void Allshow(int a)
 {
	TV T;
	ifstream fin;
	cout<<"================================================================================";
	cout<<"**********************************ALL DATA**************************************";
	cout<<"================================================================================";
	if(a==2)
		fin.open("Shows.dat",ios::binary|ios::beg);
	else if(a==1)
		fin.open("Movies.dat",ios::binary|ios::beg);
	while(fin.read((char*)&T,sizeof(T)))
	{
		cout<<"\nName :"<<T.Tname;
		cout<<"\nCode :"<<T.Tcode;
		for(int i=0;i<3;i++)
		{
			cout<<"\nCATEGORY";
			if((i+1)==1)
				cout<<" UPCOMING      "<<T.Tcategory[i];
			else if((i+1)==2)
				cout<<" TRENDING      "<<T.Tcategory[i];
			else if((i+1)==3)
				cout<<" ALL THE BEST  "<<T.Tcategory[i];


		}
		cout<<endl;
	 }
	fin.close();
	getch();
 }
//=============================================================================
//FUNCTION FOR CHECKING INPUT
//=============================================================================

 int inputchoice()
 {
	int c;char excess;
	cin>>c;
	cin.get(excess);
	if(cin.fail()||excess!='\n')
	{
		cin.clear();
		cin.ignore(15000,'\n');
		cout<<"\nINVALID IMPUT PLEASE TRY AGAIN "<<endl;
		c=inputchoice();
	}
	else
		return c;
	//return -1;
 }
//=============================================================================
//FUNCTION FOR ACCESSING USER DATABASE
//=============================================================================

 void Udatabase()
 {
	int ch,pos;
	User u;
	char ans;
	do
	{
		clrscr();
		cout<<"================================================================================";
		cout<<"*****************************USER FILE OPTIONS**********************************";
		cout<<"================================================================================";
		cout<<"\n1)SHOW ALL USERS ";
		cout<<"\n2)DELETE ALL USERS ";
		cout<<"\n3)MODIFY A USER ";
		cout<<"\n4)DELETE A USER ";
		cout<<"\n5)BACK "<<endl;
		ch=inputchoice();;
		switch (ch)
		{
			case 1:u.Read();
			       break;
			case 2:cout<<"\nARE YOU SURE YOU WANT TO DELETE ALL USERS (Y/N)";
			       cin>>ans;
			       if(ans=='y'||ans=='Y')
			       {
					remove("User.dat");
					cout<<"Data deleted";
					fstream f;
					f.open("User.dat",ios::binary);
					f.close();
					getch();
			       }
			       break;
			case 3:pos=Search();
			       u.Modify();
			       ofstream fio;
			       fio.open("User.dat",ios::binary|ios::out);
			       fio.seekp(pos);
			       fio.write((char*)&u,sizeof(u));
			       break;
			case 4:pos=Search();
			       u.Delete(pos);
			       break;
			case 5:break;
			default:cout<<"\nINVALID CHOICE";
		};
	}while(ch!=5);

}

//=============================================================================
//Void Main
//=============================================================================

 void main()
 {
	clrscr();
	int pos,choice,go;
	char ans;
	gotoxy(1,7);
	cout<<"==============================================================================="
	    <<"   -----   |      |   -----   |                     |   ----     -----  |     | "
	    <<"  |        |      |  |     |   |         -         |   |     |  |     |  |   |  "
	    <<"  |        |      |  |     |    |       | |       |    |     |  |     |   | |   "
	    <<"	  -----   |------|  |     |     |     |   |     |     |-----   |     |    |    "
	    <<"	       |  |      |  |     |      |   |     |   |      |     |  |     |   | |   "
	    <<"	       |  |      |  |     |       | |       | |       |     |  |     |  |   |  "
	    <<"	  -----   |      |   -----         -         -         -----    -----  |     |  "
	    <<"===============================================================================";
	cout<<"\nPresented To You By Vaibhav And Rishabh ";
	getch();
	do
	{
		clrscr();
		User U;
		gotoxy(1,7);
		cout<<"================================================================================";
		cout<<"**********************************SHOWBOX***************************************";
		cout<<"================================================================================";
		cout<<"\tMENU ";
		cout<<"\nKINDLY SELECT FROM THE GIVEN OPTIONS :-";
		cout<<"\n\t1)NEW USER";
		cout<<"\n\t2)EXISTING USER";
		cout<<"\n\t3)BROWSE";
		cout<<"\n\t4)ACCESS TV'S DATABASE";
		cout<<"\n\t5)ACCESS USER'S DATABASE ";
		cout<<"\n\t6)EXIT\n";
		choice=inputchoice();
		switch(choice)
		{
			case 1: cout<<"\nLOADING...............";
				delay(500);
				go=Newuser();
				if(go==2)
					goto lb;
				break;
			case 2: cout<<"\nLOADING...............";
				delay(500);
				lb:
				Euser();
				break;
			case 3: cout<<"\nLOADING...............";
				delay(500);
				cout<<"\nGuest..............";
				GBrowse();
				break;
			case 4: cout<<"\nLOADING...............";
				delay(500);
				cout<<"\nModify.............";
				Talter();
				break;
			case 5: cout<<"\nLOADING...............";
				delay(500);
				clrscr();
				cout<<"\nALL USERS.......... ";
				Udatabase();
				break;
			case 6: cout<<"\nEXITING...............";
				delay(500);
				exit(1);
				break;
			default:cout<<"\nInvalid Option";
				getch();
		};
	}while(choice!=6);
	getch();
}
//=============================================================================
//=============================THE END=========================================
//=============================================================================
