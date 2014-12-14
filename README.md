Z3
==

Main
#include <iostream>
#include <cstdlib>
#include <cstring>
#include <ctime>
using namespace std;

struct telement{
	int ai, bi, ci, di, ei;
    string ime_prezime;
    char spol;
    short d, m, g;
};

//#include "red_polje.h"
#include "red_pokazivac.h"

int sadasnja_godina = 0;
telement* stanja_klijenata;
bool generiraj(int generirano){
	telement elem;
	stanja_klijenata = new telement[generirano];
	
	for(int i = 0; i < generirano; i++)
	{
		for(int j=0; j < generirano; j++){
			stanja_klijenata[i].ai = rand()%491+10;
			stanja_klijenata[i].bi = rand()%721+80;
			stanja_klijenata[i].ci = rand()%5+1;
			stanja_klijenata[i].di = rand()%5+1;
		}
		for(int i=0; i < generirano; i++)
			cout << i+1 << ". generirana cetvorka: " << stanja_klijenata[i].ai << ", " << stanja_klijenata[i].bi << ", " << stanja_klijenata[i].ci << ", "
			<< stanja_klijenata[i].di << "." << endl;
		cout << endl;
	}
	
	if(generirano > 0)
		return true;
	else
		return false;
}

void sortiranje_prioriteti(que *R){
    que *pomR1 = InitQ(pomR1), *pomR2 = InitQ(pomR2);
    telement klijent;
	
    for(int i=1; i <= 4; i++)
	{
        while(!IsEmptyQ(R))
		{
            klijent = FrontQ(R);
            DeQueueQ(R);
            if(klijent.ci == i) 
				EnQueueQ(klijent, pomR1);
            else 
				EnQueueQ(klijent, pomR2);
        }
        while(!IsEmptyQ(pomR2))
		{
            klijent = FrontQ(pomR2);
            DeQueueQ(pomR2);
            EnQueueQ(klijent, R);
    	}
    }
    while(!IsEmptyQ(pomR1))
	{
        klijent = FrontQ(pomR1);
        DeQueueQ(pomR1);
        EnQueueQ(klijent, R);
    }
}

string ispis_usluge(telement elem){
	string pri,usl;

	if(elem.ci == 1) pri = "Prioritet klijenta: VIP klijenti";
	else if(elem.ci == 2) pri = "Prioritet klijenta: Invalidi";
	else if(elem.ci == 3) pri = "Prioritet klijenta: Trudnice";
	else if(elem.ci == 4) pri = "Prioritet klijenta: Umirovljenici";
	else if(elem.ci == 5) pri = "Prioritet klijenta: Ostali klijenti";

	if(elem.di == 1) usl = "Vrsta usluge: Predaja / preuzimanje posiljke";
	else if(elem.di == 2) usl = "Vrsta usluge: Placanje racuna";
	else if(elem.di == 3) usl = "Vrsta usluge: Uplata lutrije";
	else if(elem.di == 4) usl = "Vrsta usluge: Western union";
	else if(elem.di == 5) usl = "Vrsta usluge: Evotv";
	
	return pri + "\n" + usl;
}

bool dodaj_klijente(que* R, int br){
	telement klijent;
	que* pom = InitQ(pom);
	int starost, danasnja_godina = 2014;
	bool horoskopski_znak = false, pronadeni_klijenti = false;
	
	for(int i=0; i < br; i++)
		for(int j=0; j < 5; j++){
			do{
				
				cout << "\nUnesite spol klijenta (m - musko, z - zensko): ";
				cin >> klijent.spol;
				
				if(klijent.spol != 'm' && klijent.spol != 'z') 
					cout << "Morate unesti m ili z kako bi unos bio ispravan!" << endl;			
			}while(klijent.spol != 'm' && klijent.spol != 'z');
			
			if(klijent.spol == 'm'){
				cout << "Ime i prezime klijenta: ";
				getline(cin, klijent.ime_prezime);
			}
			else{
				cout << "Ime i prezime klijentice: ";
				getline(cin, klijent.ime_prezime);
			}
			cout << "Unesite datum rodenja klijenta ->" << endl;
			cout << "Dan: ", cin >> klijent.d;
			cout << "Mjesec: ", cin >> klijent.m; 
			cout << "Godina: ", cin >> klijent.g;
			starost = danasnja_godina - klijent.g;
			klijent.ai = stanja_klijenata[i].ai;
			klijent.bi = stanja_klijenata[i].bi;
			klijent.ci = stanja_klijenata[i].ci;
			klijent.di = stanja_klijenata[i].di;
			cout << "------------" << endl;			

			if(klijent.spol == 'm' && klijent.ci == 3) klijent.ci = 5;
			if(starost > 65) klijent.ci = 4;
			else if(starost < 7) klijent.ci = 5;
			cout << ispis_usluge(klijent) << endl;
			cout << "--------------------------" << endl << endl;

			EnQueueQ(klijent, R);
		}
		sortiranje_prioriteti(R);
		
		cout << "Uplata lutrije od klijenata mladih od 40, horoskopskog znaka Jarac: " << endl;
		cout << "------------------" << endl;		
		while(!IsEmptyQ(R)){
			klijent = FrontQ(R);
			
			starost = sadasnja_godina - klijent.g;
			if((klijent.d >= 22 && klijent.m == 12) || (klijent.d <= 20 && klijent.m == 1)) horoskopski_znak = true;
			else horoskopski_znak = false;
			
			if(klijent.di == 3 && starost < 40 && horoskopski_znak){
				pronadeni_klijenti = true;
				
				cout << "Ime i prezime: " << klijent.ime_prezime << endl;
				cout << "Datum rodenja: " << klijent.d << "." << klijent.m << "." << klijent.g << endl;
				cout << "Godine: " << starost << endl;
				cout << ispis_usluge(klijent);
				cout << "\n---------" << endl;
			}
			
			EnQueueQ(klijent, pom);
			DeQueueQ(R);
		}
			
		while(!IsEmptyQ(pom)){
			klijent = FrontQ(pom);
			DeQueueQ(pom);
			EnQueueQ(klijent, R);
		}
		
		return pronadeni_klijenti;		
}

void izlazak_trudnica(que* R){
	telement klijent;
	bool ispis = false;
	que* pomocni = InitQ(pomocni);
	
	while(!IsEmptyQ(R)){
		klijent = FrontQ(R);
		EnQueueQ(klijent, pomocni);
		DeQueueQ(R);
	}
	
	cout << "Trudnice koje su dosle vezano uz uslugu Evotv-a" << endl;	
	cout << "-------------" << endl;
	while(!IsEmptyQ(pomocni)){
		klijent = FrontQ(pomocni);
		DeQueueQ(pomocni);
		
		if(klijent.ci == 3 && klijent.bi >= 550 && klijent.di == 5){
			ispis = true;
			
			cout << "Ime i prezime: " << klijent.ime_prezime << endl;
			cout << "Spol: " << klijent.spol << endl;
			cout << "Datum rodenja: " << klijent.d << "." << klijent.m << "." << klijent.g << "." << endl;
			cout << "Prije izlaszka trudnica je cekala: " << klijent.ai << " vremena." << endl;
			cout << "--------------" << endl << endl;
		}
		else EnQueueQ(klijent, R);
	}
	if(!ispis) 
		cout << "Ne postoji niti jedna trudnica koja ceka na uslugu Evotv-a." << endl << endl;
	
	cout << "------- Cekaonica ---------" << endl;
	while(!IsEmptyQ(R)){
		klijent = FrontQ(R);
		DeQueueQ(R);
		EnQueueQ(klijent, pomocni);
	}
	while(!IsEmptyQ(pomocni)){
		klijent = FrontQ(pomocni);
						
		cout << "Ime i prezime klijenta: " << klijent.ime_prezime << endl;
		cout << "Spol: " << klijent.spol << endl;
		cout << "Datum rodenja: " << klijent.d << "." << klijent.m << "." << klijent.g << "." << endl;
		cout << ispis_usluge(klijent) << endl;
		cout << "--------------" << endl << endl;
		
		DeQueueQ(pomocni);
		EnQueueQ(klijent, R);
	}
}

int main(){
	srand(time(NULL));
	rand();
	que* R = InitQ(R);
	int generirano, odabir;
	bool red_postoji = false, alokacija = false;
	
	do{
		cout << "----- Izbornik ------" << endl;
		cout << "1. Generiranje ntorki" << endl;
		cout << "2. Podaci o klijentima" << endl;
		cout << "3. Trudnice napustaju red radi predugog cekanja" << endl;
		cout << "0. Izlaz iz programa." << endl;
		cout << "---------------------" << endl;
		do{
			cout << "Odaberite opciju: ";
			cin >> odabir;
			cin.ignore();
			
			if(odabir < 0 || odabir > 4) 
				cout << "Unijeli ste krivu vrijednost. Pokusajte ponovo." << endl << endl;
		
		}while(odabir < 0 || odabir > 4);
		cout << endl;
			
		switch(odabir){
			case 1:
				cout << "Koliko ntorki zelite generirati? (Unesite broj): ";
				cin >> generirano;
				cout << "----------------------" << endl;

				alokacija = generiraj(generirano);
				if(alokacija) 
					red_postoji = true;
				break;
			case 2: 
				if(red_postoji)
				{
					if(!dodaj_klijente(R, generirano));
						cout << "Nema klijenata mladih od 40 godina horoskopskog znaka Jarca." << endl << endl;
				} 
				else 
					cout << "Potrebno je kreirati i popuniti red." << endl << endl;
				break;
			case 3: 
				if(red_postoji) 
					izlazak_trudnica(R);
				else 
					cout << "Potrebno je kreirati i popuniti red." << endl << endl;
				break;

			default: break;
		}
	}while(odabir);
}
