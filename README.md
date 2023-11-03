# hospital-management
This is my hospital management project
#include <stdio.h>
#include <stdlib.h>
#include<time.h>

struct patient
{
    int id;
    char patientName[50];
    char patientAddress[50];
    char disease[50];
    char date[12];
}p;

struct doctor
{
    int id;
    char Name[50];
    char Address[50];
    char specialist[50];
    char date[12];
}d;

FILE*fp;

int main()
{
    int ch;
    while(1)
    {
        system("cls");
        printf("*************************HOSPITAL MANAGEMENT SYSTEM*************************\n\n");
        printf("1: ADMIT PATIENT\n");
        printf("2: PATIENT LIST\n");
        printf("3: DISCHARGE PATIENT\n");
        printf("4: ADD DOCTOR\n");
        printf("5:DOCTORS LIST\n");
        printf("0: EXIT\n");
        scanf("%d",&ch);

        switch(ch)
        {
            case 0:
            exit(0);
            case 1:
                admitPatient();
                break;
            case 2:
                patientList();
                break;
            case 3:
                dischargePatient();
            case 4:
                addDoctor();
                break;
            case 5:
                doctorList();
                break;
            default:
                printf("INVALID CHOICE...\n\n");
        }
        printf("\n\nPress Any Key To Continue");
        getch();
    }
    return 0;
}

void admitPatient()
{
    char myDate[12];
    time_t t=time(NULL);
    struct tm tm =*localtime(&t);
    sprintf(myDate,"%02d/%02d/%d",tm.tm_mday,tm.tm_mon+1,tm.tm_year+1900);
    strcpy(p.date,myDate);

    fp=fopen("patient.txt","ab");
    printf("ENTER PATIENT ID:");
    scanf("%d",&p.id);

    printf("ENTER PATIENT NAME:\n");
    fflush(stdin);
    gets(p.patientName);

    printf("ENTER PATIENT ADDRESS:\n");
    scanf("%c",&p.patientAddress);

    printf("ENTER PATIENT DISEASE:\n");
    fflush(stdin);
    gets(p.disease);


    printf("\nPatient Added Successfully");

    fwrite(&p,sizeof(p),1,fp);
    fclose(fp);
}


void patientName()
{
    system("cls");
    printf("--------------------------------PATIENT LIST--------------------------------\n\n");
    printf("%-10s %-30s %-30s %-20s %s \n","id" "Patient name","Address","Disease","Date");
    printf("------------------------------------------------------------------------------------------------\n");
    fp=fopen("patient.txt","rb");
    while(fread(&p,sizeof(p),1,fp)==1)
    {
        printf("%-10s %-30s %-30s %-20s %s\n",p.id,p.patientName,p.patientAddress,p.disease,p.date);
    }
    fclose(fp);

}


void patientList()
{
    system("cls");
    printf("-------PATIENT LIST-------\n\n");
    printf("%-10s %-30s %-30s %-20s %s\n","id","Patient Name","Address","Disease","Date");
    printf("-----------------------------------------------------------------------------------------------------\n");

    fp=fopen("patient.txt","rb");
    while(fread(&p,sizeof(p),1,fp)==1)
    {
        printf("%-10d %-30s %-30s %-20s %s\n",p.id,p.patientName,p.patientAddress,p.disease,p.date);
    }
    fclose(fp);
}


void dischargePatient()
{
    int id,f=0;
    system("cls");
    printf("----------------DISCHARGE PATIENT----------------\n\n");
    printf("ENTER PATIENT ID TO DISCHARGE:\n");
    scanf("%d",&id);

    FILE *ft;
    fp=fopen("patient.txt","rb");
    ft=fopen("temp.txt","wb");

    while(fread(&p,sizeof(p),1,fp)==1)
    {
        if(id==p.id)
        {
            f=1;
        }
        else
        {
            fwrite(&p,sizeof(p),1,ft);
        }
    }
    if(f==1)
    {
        printf("\n\nPatient Discharged Successfully:");
    }
    else
    {
        printf("\n\nRecord not found!");
    }
    fclose(fp);
    fclose(ft);


    remove("patient.txt");
    rename("temp.txt","patient.txt");
}

void addDoctor()
{
    system("cls");
    char myDate[12];
    time_t t=time(NULL);
    struct tm tm=*localtime(&t);
    sprintf(myDate,"%02d/%02d/%d",tm.tm_mday,tm.tm_mon+1,tm.tm_year+1900);
    strcpy(d.date,myDate);

    int f=0;
    //system("cls");
    printf("--------------ADD DOCTOR--------------\n");
    fp=fopen("doctor.txt","ab");
    printf("ENTER DOCTOR ID:\n");
    scanf("%d",&d.id);

    printf("ENTER DOCTOR NAME:\n");
     fflush(stdin);
    gets(d.Name);

    printf("ENTER DOCTOR ADDRESS:\n");
    scanf("%c",&d.Address);

    printf("DOCTOR SPECIALIZE IN:\n");
    fflush(stdin);
    gets(d.specialist);
    printf("\nDoctor Added Successfully\n");

    fwrite(&d,sizeof(d),1,fp);
    fclose(fp);

}

/*void doctorList()
{
    system("cls");
    printf("--------------DOCTOR LIST--------------\n");
    printf("%-10s %-30s %-30s %-20s %s\n","id","Doctor Name","Address","specialize","Date");
    printf("-----------------------------------------------------------------------------------------------------\n");

    fp=fopen("doctor.txt","rb");
    while(fread(&d,sizeof(d),1,fp)==1)
    {
        printf("%-10s %-30s %-30s %-20s %s\n",d.id,d.Name,d.Address,d.specialist,d.date);
    }
    fclose(fp);
}*/

void doctorList()
{
    system("cls");
    printf("-------DOCTOR LIST-------\n\n");
    printf("%-10s %-30s %-30s %-20s %s\n","id","Doctor Name","Address","specialize","Date");
    printf("-----------------------------------------------------------------------------------------------------\n");

    fp=fopen("doctor.txt","rb");
    while(fread(&d,sizeof(d),1,fp)==1)
    {
        printf("%-10d %-30s %-30s %-20s %s\n",d.id,d.Name,d.Address,d.specialist,d.date);
    }
    fclose(fp);
}

