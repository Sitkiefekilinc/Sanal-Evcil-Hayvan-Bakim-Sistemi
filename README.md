#include <iomanip> 
#include <cstring> 
#include <stdlib.h>
#include <conio.h>  //Sanal Evcil Hayvan Bakım Sistemi
#include <stdio.h> 
#include <fstream>                  
#include <iostream>

using namespace std;

struct musteri
{
	char ad[80];
	char soyad[80];
	char eposta[80];
	char tc_no[11];
	char cep_no[11];	
};



struct evcil_h
{
	char h_turu[80];
	char h_cinsi[80];
	char h_cinsiyeti[5];
	char h_yasi[2];
	char b_paket[5];
	musteri mus;
};

void musterikayit();
void musterilisteleme();
void musteriarama();
void musterisilme();
void musterikayitduzeltme();
int main(){
	
	char anamenu;
	do{
	system("cls");
	
	cout << "|-------Hosgeldiniz------|" << endl ;
	cout << "|      Secim Yapiniz     |" << endl ;
	cout << "|  1- Yeni Musteri kaydi |" << endl ;
	cout << "|  2- kayit listesi      |" << endl ;
	cout << "|  3- kayit sorgulama    |"<< endl;
	cout << "|  4- kayit silme        |"<< endl;
	cout << "|  5- Kayit  Duzenleme   |"<< endl;
	cout << "|------------------------|" << endl ;
    char secim;
	cin>> secim;
	
	switch(secim) 
	{
		case '1' : 
		{
		musterikayit();
		break;	
		}
		case '2' : 
		{
		 musterilisteleme();
		 break;
		}
		case '3' : 
		{
		musteriarama();
		 break;
		}
		case '4' : 
		{
		musterisilme();
		 break;
		}
		case '5' : 
		{
		musterikayitduzeltme();
		 break;
		}
	}
	
	cout << "Anamenuye Donmek icin:A | Cikmak icin:c ye basınız" <<endl ; 
	anamenu=getche();
	
    }while(anamenu=='a');
     

return 0;
}


evcil_h evc;
void musterikayit()
{
	ofstream m_kayit("musteri.dat",ios::binary |ios::app);
	char secim,secim2;
	int adet=0;
	
	do{
	cout << "Adinizi giriniz:" << endl;
	cin>> evc.mus.ad;
	cout << "Soyadinizi Giriniz:" << endl;
	cin>> evc.mus.soyad;
	cout << "E-postanizi giriniz Giriniz:" << endl;
	cin>> evc.mus.eposta;
	cout << "Tc kimlik numaranizi giriniz:" << endl;
	cin>>evc.mus.tc_no;
	cout<<"Cep telefon numarasini giriniz:"<<endl;
	cin>>evc.mus.cep_no;
	cout<<"Evcil hayvaninizin turunu giriniz:"<<endl;
	cin>>evc.h_turu;
	cout<<"Evcil hayvaninin cinsini giriniz:"<<endl;
	cin>>evc.h_cinsi;
	cout<<"Evcil hayvaninizin cinsiyetini giriniz:"<<endl;
	cin>>evc.h_cinsiyeti;
	cout<<"Evcil hayvanizin yasini giriniz:"<<endl;
	cin>>evc.h_yasi;
	cout<<"Hangi bakim paketini secmek istersiniz?"<<endl;
	cout<<"1-Baslangic bakim paketi"<<endl;
	cout<<"2-Orta bakim paketi"<<endl;
	cout<<"3-Yuksek bakim paketi"<<endl;
	cin>>evc.b_paket;
	cout<<"Kaydi kaydetmek istiyorsaniz e/E tusuna basiniz?"<<endl;
	secim=getche();
	if(secim=='e' ||secim=='E' )
	{
	m_kayit.write((char*)&evc, sizeof(evc));
	adet++;	
    }
    cout<<"Yeni kayit eklemek istiyor musunuz? e/E"<<endl;
    cin>>secim2;
    
	}while(secim2=='e' || secim=='E');
	
	cout << adet << " adet musteri Eklendi.." << endl;
	
	m_kayit.close();
	
	
}

void musterilisteleme()
{
	
	ifstream oku("musteri.dat",ios::binary | ios::app);
	
	oku.seekg(0,ios::end);
	int kayits=oku.tellg()/sizeof(evc);
	cout << "Toplam Musteri Kayit Sayisi:"<< kayits << endl;
	
	if(kayits>0)
	{
		for(int i=0; i<kayits;i++)
		{
			oku.seekg(i*sizeof(evc));
			oku.read((char*)&evc, sizeof(evc));	
		
			cout << i+1 << ". Musterinin bilgileri"<< endl;
			cout << "Musterinin Adi: "<< evc.mus.ad <<endl ;
			cout << "Musterinin Soyadi: "<<evc.mus.soyad <<endl ;
			cout << "Musterinin tc kimlik numarasi : "<< evc.mus.tc_no <<endl ;
			cout << "Musterinin cep telefonu : "<< evc.mus.eposta <<endl ;
			cout << "Evcil hayvanin turu : "<< evc.h_turu <<endl ;
			cout << "Evcil hayvanin cinsi : "<<evc.h_cinsi <<endl ;
			cout << "Evcil hayvanin cinsiyeti : "<< evc.h_cinsiyeti <<endl ;
			cout << "Evcil hayvanin yasi : "<< evc.h_yasi <<endl ;
			cout << "Bakim paketi : "<< evc.b_paket <<endl ;
			
		}	
	}
	else
	cout << "Kayit Bulunamadi..." << endl;

	oku.close();
}

void musteriarama()
{
	
	ifstream oku("musteri.dat",ios::binary |ios::app);
	
	oku.seekg(0,ios::end);
	int kayits=oku.tellg()/sizeof(evc);
	
	char a_tc[80];
	cout<<"Tc kimlik numarasini giriniz"<<endl;
	cin>>a_tc;
	if(kayits>0)
	{
		for(int i=0; i<kayits;i++)
		{
			oku.seekg(i*sizeof(evc));
			oku.read((char*)&evc, sizeof(evc));	
		if(strcmp(evc.mus.tc_no,a_tc)==0)
		{
			cout <<"Buluan Musterinin bilgileri"<< endl;
			cout << "Musterinin Adi: "<< evc.mus.ad <<endl ;
			cout << "Musterinin Soyadi: "<<evc.mus.soyad <<endl ;
			cout << "Musterinin tc kimlik numarasi : "<< evc.mus.tc_no <<endl ;
			cout << "Musterinin cep telefonu : "<< evc.mus.eposta <<endl ;
			cout << "Evcil hayvanin turu : "<< evc.h_turu <<endl ;
			cout << "Evcil hayvanin cinsi : "<<evc.h_cinsi <<endl ;
			cout << "Evcil hayvanin cinsiyeti : "<< evc.h_cinsiyeti <<endl ;
			cout << "Evcil hayvanin yasi : "<< evc.h_yasi <<endl ;
			cout << "Bakim paketi : "<< evc.b_paket <<endl ;
     	}
		}	
	}
	else
	cout << "Kayit Bulunamadi..." << endl;

	oku.close();
}

void musterisilme()
{
		char secim4;
	bool var=false;
	char a_tc[80];
	ifstream oku("musteri.dat",ios::binary);
	
	oku.seekg(0,ios::end);
	int kayits=oku.tellg()/sizeof(evc);

	
	cout<<"Tc kimlik numarasini giriniz"<<endl;
	cin>>a_tc;

		for(int i=0; i<kayits;i++)
		{
			oku.seekg(i*sizeof(evc));
			oku.read((char*)&evc, sizeof(evc));	
		if(strcmp(evc.mus.tc_no,a_tc)==0)
		{
			cout <<"Silinecek musteri bu mu?"<< endl;
			cout << "Musterinin Adi: "<< evc.mus.ad <<endl ;
			cout << "Musterinin Soyadi: "<<evc.mus.soyad <<endl ;
			cout << "Musterinin tc kimlik numarasi : "<< evc.mus.tc_no <<endl ;
			cout << "Musterinin cep telefonu : "<< evc.mus.eposta <<endl ;
			cout << "Evcil hayvanin turu : "<< evc.h_turu <<endl ;
			cout << "Evcil hayvanin cinsi : "<<evc.h_cinsi <<endl ;
			cout << "Evcil hayvanin cinsiyeti : "<< evc.h_cinsiyeti <<endl ;
			cout << "Evcil hayvanin yasi : "<< evc.h_yasi <<endl ;
			cout << "Bakim paketi : "<< evc.b_paket <<endl ;
			cout<<endl;
			cout<<"Bu musteriyi silmek istedi,ğinizden emin misiniz?"<<endl;
			secim4=getche();
			if(secim4=='h'||secim4=='h')
			{
					
	    	ofstream m_silme("y_mus.dat",ios::app | ios::binary);
	    	m_silme.write((char*)&evc,sizeof(evc));
	    	m_silme.close();
	    	
				
			}
			else if(secim4=='e'||secim4=='E')
			{
				var=true;
			}
     	}
	    
	    else 
	    {
	    	
	    	ofstream m_silme("y_mus.dat",ios::app | ios::binary);
	    	
	    	
	    	m_silme.write((char*)&evc,sizeof(evc));
	    	m_silme.close();
	    	
	    	
		}
	
		
		
		
		}	
			oku.close();
		if(var)
		{
			remove("musteri.dat");
			rename("y_mus.dat","musteri.dat");
			cout<<"Kayit silindi"<<endl;
		}
		else
		remove("y_musteri.dat");


}


void musterikayitduzeltme()
{
	
	ifstream oku("musteri.dat",ios::binary);
	
	oku.seekg(0,ios::end);
	int kayits=oku.tellg()/sizeof(evc);
	char secim4;
	bool var=false;
	char a_tc[80];
	
	cout<<"Tc kimlik numarasini giriniz"<<endl;
	cin>>a_tc;
	if(kayits>0)
	{
		for(int i=0; i<kayits;i++)
		{
			oku.seekg(i*sizeof(evc));
			oku.read((char*)&evc, sizeof(evc));	
		if(strcmp(evc.mus.tc_no,a_tc)==0)
		{
			cout <<"Duzeltilecek musteri bu mu?"<< endl;
			cout << "Musterinin Adi: "<< evc.mus.ad <<endl ;
			cout << "Musterinin Soyadi: "<<evc.mus.soyad <<endl ;
			cout << "Musterinin tc kimlik numarasi : "<< evc.mus.tc_no <<endl ;
			cout << "Musterinin cep telefonu : "<< evc.mus.eposta <<endl ;
			cout << "Evcil hayvanin turu : "<< evc.h_turu <<endl ;
			cout << "Evcil hayvanin cinsi : "<<evc.h_cinsi <<endl ;
			cout << "Evcil hayvanin cinsiyeti : "<< evc.h_cinsiyeti <<endl ;
			cout << "Evcil hayvanin yasi : "<< evc.h_yasi <<endl ;
			cout << "Bakim paketi : "<< evc.b_paket <<endl ;
			cout<<endl;
			cout<<"Bu musteriyi duzenlemek istediginizden emin misiniz?"<<endl;
			secim4=getche();
			if(secim4=='h'||secim4=='h')
			{
					
	    	ofstream m_silme("y_mus.dat",ios::app | ios::binary);
	    	m_silme.write((char*)&evc,sizeof(evc));
	    	m_silme.close();
	    	
				
			}
			else if(secim4=='e'||secim4=='E')
			{
				var=true;
				ofstream m_silme("y_mus.dat",ios::app | ios::binary);
				cout << "Adinizi giriniz: " << endl;
	cin>> evc.mus.ad;
	cout << "Soyadinizi Giriniz:" << endl;
	cin>> evc.mus.soyad;
	cout << "E-postanızı giriniz Giriniz:" << endl;
	cin>> evc.mus.eposta;
	cout << "Tc kimlik numaranizi giriniz:" << endl;
	cin>>evc.mus.tc_no;
	cout<<"Cep telefon numaranizi giriniz:"<<endl;
	cin>>evc.mus.cep_no;
	cout<<"Evcil hayvaninizin turunu giriniz:"<<endl;
	cin>>evc.h_turu;
	cout<<"Evcil hayvaninizin cinsi:"<<endl;
	cin>>evc.h_cinsi;
	cout<<"Evcil hayvaninizin cinsiyetini giriniz: "<<endl;
	cin>>evc.h_cinsiyeti;
	cout<<"Evcil hayvaninizin yasini giriniz:"<<endl;
	cin>>evc.h_yasi;
	cout<<"Hangi bakim paketini secmek istersiniz?"<<endl;
	cout<<"1-Baslangic bakim paketi"<<endl;
	cout<<"2-Orta bakim paketi"<<endl;
	cout<<"3-Yuksek bakim paketi"<<endl;
	cin>>evc.b_paket;
				
					m_silme.write((char*)&evc,sizeof(evc));
	    	m_silme.close();
				
	
				
				
			}
     	}
	    
	    else 
	    {
	    	
	    	ofstream m_silme("y_mus.dat",ios::app | ios::binary);
	    	
	    	
	    	m_silme.write((char*)&evc,sizeof(evc));
	    	m_silme.close();
	    	
	    	
		}
	
		
		
		
		}	
			oku.close();
		if(var)
		{
			remove("musteri.dat");
			rename("y_mus.dat","musteri.dat");
			cout<<"Kayit duzeltildi"<<endl;
		}
		else
		remove("y_musteri.dat");
	}
	else
	cout << "Kayit Bulunamadi..." << endl;


}








