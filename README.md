# account_mgnt_sys
Account Management System using C programming.
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<conio.h>
struct account{
    char first_name[20];
    char last_name[20];
    char user_name[20];
    char phone[15];
    char address[30];
    char DOB[15];
    char gender;
    char password[15];
};
void divider()
{
    for (int i = 0; i < 50; i++)
    {
        printf("-");
    }
    printf("\n");
}
int menu();
void sign_up();
void log_in();
void change_pass();
void delete_account();
char* takepassword(char []);
int main()
{
    while(1)
    {
        system("cls");
        switch (menu())
        {
        case 1:
            sign_up();
            break;
        case 2:
            log_in();
            break;
        case 3:
            change_pass();
            break;
        case 4:
            delete_account();
            break;
        case 5:
            exit(0);
        default:
            printf("INVALID NUMBER.\n");
            break;
        }
    }
    return 0;
}
int menu()
{
    int c;
    printf("***************MAIN MENU***************\n");
    divider();
    printf("1.SIGN UP.\n");
    printf("2.LOG IN.\n");
    printf("3.CHANGE PASSWORD.\n");
    printf("4.DELETE ACCOUNT.\n");
    printf("5.EXIT.\n");
    divider();
    printf("Enter your choice. ");
    scanf("%d",&c);
    return c;
}
void sign_up()
{
    struct account s;
    system("cls");
    char nick_name[20];
    char pass2[20];
    printf("CREATING YOUR ACCOUNT....\n");
    divider();
    printf("Enter your first name: ");
    scanf("%s",s.first_name);
    printf("Enter your last name: ");
    scanf("%s",s.last_name);
    printf("Enter your user name: ");
    scanf("%s",s.user_name);
    printf("Enter your mobile number: ");
    scanf("%s",s.phone);
    printf("Enter your address: ");
    scanf("%s",s.address);
    printf("Enter your Date of birth(YYYY-MM-DD): ");
    scanf("%s",s.DOB);
    start:
    printf("Enter your Gender(M/F): ");
    scanf(" %c",&s.gender);
    if (s.gender != 'M' && s.gender != 'F')
    {
        printf("INVALID GENDER!\n");
        goto start;
    }
    top:
    printf("Enter your password: ");
    takepassword(s.password);
    printf("\n");
    printf("Confirm your password: ");
    takepassword(pass2);
    printf("\n");
    if(strcmp(pass2,s.password) !=0)
    {
        printf("Passwords doesn't match try again.\n");
        goto top;
    }
    else
    {
        FILE *fp = fopen("D:\\account.txt", "w");
        printf("Enter security question below for changing password: \n");
        printf("Your nickname: ");
        scanf("%s",nick_name);
        fwrite(&s, sizeof(struct account), 1, fp);
        fclose(fp);
        printf("ACCOUNT CREATED SUCCESFULLY.");
    }
}
char* takepassword(char pass[])
{
    int i = 0;
    char ch;
    while (1)
    {
        ch = getch();
        if (ch == 13)
        {
            pass[i] = '\0';
            break;
        }
        else if (ch == 8)
        {
            if (i > 0)
            {
                i--;
                printf("\b \b");
            }
        }
        else if (ch == 9 || ch == 32)
        {
            continue;
        }
        else
        {
            pass[i++] = ch;
            printf("*");
        }
    }
    return pass;
}
void log_in()
{
    system("cls");
    char user[30], log_pass[20];

    struct account s;
    FILE *fp = fopen("D:\\account.txt", "r");
    fread(&s, sizeof(struct account), 1, fp);

name:
    printf("Enter Username : ");
    scanf("%s", user);
    if (strcmp(s.user_name, user))
    {
        printf("\nPLEASE ENTER CORRECT USERNAME\n ");
        goto name;
    }

    else
    {
    logpass:
        printf("Enter password : ");
        takepassword(log_pass);
        if (strcmp(s.password, log_pass))
        {
            printf("\nINCORRET PASSWORD \n");
            goto logpass;
        }
        else
        {
            system("cls");
            printf("\n*****************WELCOME %s*****************\n", s.first_name);
            divider();

            printf("\nYour Details \n");
            divider();
            printf("First name   : %s\n", s.first_name);
            printf("Last name    : %s\n", s.last_name);
            printf("Phone number : %s\n", s.phone);
            printf("Address      : %s\n",s.address);
            printf("DOB          : %s\n",s.DOB);
            printf("Gender       : %c\n", s.gender);
            printf("Username     : %s\n", s.user_name);
            printf("Password     : %s\n", s.password);
        }
        getch();
    }
    fclose(fp);
}
void change_pass()
{
    system("cls");
    char pass2[20];
    struct account s;
    FILE *fp = fopen("D:\\account.txt", "r");
    fread(&s, sizeof(s), 1, fp);
    top:
    printf("Enter new password : ");
    takepassword(s.password);
    printf("\n");
    printf("Confirm new password :");
    takepassword(pass2);
    printf("\n");
    if(strcmp(pass2,s.password))
    {
        printf("Passwords doesn't match try again.\n");
        goto top;
    }
    else
    {
        fp = fopen("D:\\account.txt", "w");
        fwrite(&s, sizeof(s), 1, fp);
        printf("***************PASSWORD CHANGED SUCCESSFULLY***************\n");
        getch();
    }
    fclose(fp);
}
void delete_account()
{
    FILE *fp = fopen("D:\\account.txt", "r");

    printf("ARE YOU SURE[Y/N] : ");
    char ch = getche();
    if (ch == 'Y' || ch == 'y')
    {
        fp = fopen("D:\\account.txt", "w");
        fprintf(fp, " ");
        printf("\nACCOUNT DELETED SUCCESSFULLY\n");
        getch();
    }

    else if (ch == 'N' || ch == 'n')
    {
        printf("\nACCOUNT DELETION STOPPED\n");
        getch();
    }
}
