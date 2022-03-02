//                   HEADER FILE USED IN PROJECT
//****************************************************************

#include<fstream>
#include<conio.h>
#include<stdio.h>
#include<string.h>
#include<iomanip>
#include<iostream>
using namespace std;
//***************************************************************
//                   CLASS USED IN PROJECT
//****************************************************************



class book
{
    public:
    char bno[6];
    char bname[50];
    char aname[20];
  public:
    void create_book()
    {
        cout<<"\nNEW BOOK ENTRY...\n";
        cout<<"\nEnter The book no.";
        cin>>bno;
        cout<<"\n\nEnter The Name of The Book ";
        cin>>bname;
        //gets(bname);
        cout<<"\n\nEnter The Author's Name ";
        cin>>aname;
        //gets(aname);
        cout<<"\n\n\nBook Created..";
    }

    void show_book()
    {
        cout<<"\nBook no. : "<<bno;
        cout<<"\nBook Name : ";
        puts(bname);
        cout<<"Author Name : ";
        puts(aname);
    }

    void modify_book()
    {
        cout<<"\nBook no. : "<<bno;
        cout<<"\nModify Book Name : ";
        gets(bname);
        cout<<"\nModify Author's Name of Book : ";
        gets(aname);
    }

    char* retbno()
    {
        return bno;
    }

    void report()
    {cout<<bno<<setw(30)<<bname<<setw(30)<<aname<<endl;}


};         //class for book
//***************************************************************
//        global declaration for stream object, object
//****************************************************************

fstream fp,fp1;
book bk;
//***************************************************************
//        function to write in file
//****************************************************************
void write_book()
{
    char ch;
    fp.open("book.dat",ios::out|ios::app);
    do
    {
        bk.create_book();
        fp.write((char*)&bk,sizeof(book));
        cout<<"\n\nDo you want to add more record..(y/n?)";
        cin>>ch;
    }while(ch=='y'||ch=='Y');
    fp.close();
}
//***************************************************************
//        function to read specific record from file
//****************************************************************


void display_spb(char n[])
{
    cout<<"\nBOOK DETAILS\n";
    int flag=0;
    fp.open("book.dat",ios::in);
    while(fp.read((char*)&bk,sizeof(book)))
    {
        if(strcmpi(bk.retbno(),n)==0)
        {
            bk.show_book();
             flag=1;
        }
    }
   
    fp.close();
    if(flag==0)
        cout<<"\n\nBook does not exist";
    getch();
}
//***************************************************************
//        function to modify record of file
//****************************************************************


void modify_book()
{
    char n[6];
    int found=0;
    cout<<"\n\n\tMODIFY BOOK REOCORD.... ";
    cout<<"\n\n\tEnter The book no. of The book";
    cin>>n;
    fp.open("book.dat",ios::in|ios::out);
    while(fp.read((char*)&bk,sizeof(book)) && found==0)
    {
        if(strcmpi(bk.retbno(),n)==0)
        {
            bk.show_book();
            cout<<"\nEnter The New Details of book"<<endl;
            bk.modify_book();
            int pos=-1*sizeof(bk);
                fp.seekp(pos,ios::cur);
                fp.write((char*)&bk,sizeof(book));
                cout<<"\n\n\t Record Updated";
                found=1;
        }
    }

        fp.close();
        if(found==0)
            cout<<"\n\n Record Not Found ";
        getch();
}
//***************************************************************
//        function to delete record of file
//****************************************************************
void delete_book()
{
    char n[6];
    cout<<"\n\n\n\tDELETE BOOK ...";
    cout<<"\n\nEnter The Book no. of the Book You Want To Delete : ";
    cin>>n;
    fp.open("book.dat",ios::in|ios::out);
    fstream fp2;
    fp2.open("Temp.dat",ios::out);
    fp.seekg(0,ios::beg);
    while(fp.read((char*)&bk,sizeof(book)))
    {
        if(strcmpi(bk.retbno(),n)!=0) 
        {
            fp2.write((char*)&bk,sizeof(book));
        }
    }
       
    fp2.close();
        fp.close();
        remove("book.dat");
        rename("Temp.dat","book.dat");
        cout<<"\n\n\tRecord Deleted ..";
        getch();
}
//***************************************************************
//        function to display Books list
//****************************************************************

void display_allb()
{
    fp.open("book.dat",ios::in);
    if(!fp)
    {
        cout<<"ERROR!!! FILE COULD NOT BE OPEN ";
               getch();
               return;
         }

    cout<<"\n\n\t\tBook LIST\n\n";
    cout<<"=========================================================================\n";
    cout<<"Book Number"<<setw(20)<<"Book Name"<<setw(25)<<"Author\n";
    cout<<"=========================================================================\n";

    while(fp.read((char*)&bk,sizeof(book)))
   
        bk.report();
         fp.close();
         getch();
}
//***************************************************************
//        MAIN MENU
//****************************************************************

int main()
{
    int ch2;
    cout<<"\n\n\t1.CREATE BOOK ";
    cout<<"\n\n\t2.DISPLAY ALL BOOKS ";
    cout<<"\n\n\t3.DISPLAY SPECIFIC BOOK ";
    cout<<"\n\n\t4.MODIFY BOOK ";
    cout<<"\n\n\t5.DELETE BOOK ";
    cout<<"\n\n\t6.BACK TO MAIN MENU";
    cout<<"\n\n\tPlease Enter Your Choice (1-6) ";
    cin>>ch2;
    switch(ch2)
    {
        case 1:write_book();break;
        case 2: display_allb();break;
        case 3: {
                   char num [6];
                   cout<<"\n\n\tPlease Enter The book No. ";
                   cin>>num;
                   display_spb(num);
                   break;
            }
              case 4: modify_book();break;
              case 5: delete_book();break;
             case 6: exit(0);
              default:cout<<"\a";
       }
}
//***************************************************************
//                END OF PROJECT
//***************************************************************
