# Sistem-Peminjaman-Buku
Aplikasi berbasis C++ yang mengelola peminjaman dan pengembalian buku menggunakan struktur data antrian, dengan fitur sorting dan manajemen data yang efisien.

#include <iostream>
#include <cstdlib>
#define max 5

using namespace std;

struct Buku
{
    string nama;
    string judul;
    int tanggal_pinjam;
    string nim;
    int tanggal_pengembalian;
};
Buku pinjaman;


struct Antrian
{
    int head;
    int tail;
    Buku data[max];
} antrian;

void login()
{
    string Nama, passwd;
    while (true)
    {
        cout << "Masukan Nama Anda: ";
        cin >> Nama;
        cout << "Masukan Password: ";
        cin >> passwd;
        if (passwd == "admin")
        {
            cout << "Selamat Datang " << Nama << "!!" << endl;
            system("cls");
            break;
        }
        else
        {
            cout << "Password Salah !!!" << endl;
        }
    }
}

bool kosong()
{
    return antrian.tail == antrian.head;
}

bool penuh()
{
    return antrian.tail == max;
}

void tampilData()
{
    if (!kosong())
    {
        for (int i = antrian.head; i < antrian.tail; i++)
        {
            cout << "Nama: " << antrian.data[i].nama << endl;
            cout << "NIM: " << antrian.data[i].nim << endl;
            cout << "Judul: " << antrian.data[i].judul << endl;
            cout << "Tanggal Peminjaman: " << antrian.data[i].tanggal_pinjam << endl;
            cout << "Tanggal Pengembalian: " << antrian.data[i].tanggal_pengembalian << endl
                 << endl;
        }
    }
    else
    {
        cout << "Data Kosong !!!" << endl;
    }
}

void tambahData()
{
    if (!penuh())
    {
        cout << "Masukan data:" << endl;
        cout << "Nama: ";
        cin >> pinjaman.nama;
        cout << "NIM: ";
        cin >> pinjaman.nim;
        bool judulUnik;
        do
        {
            judulUnik = true;
            cout << "Judul: ";
            cin >> pinjaman.judul;

            // Memeriksa judul duplikat
            for (int i = antrian.head; i < antrian.tail; i++)
            {
                if (antrian.data[i].judul == pinjaman.judul)
                {
                    cout << "Judul buku sudah ada, masukan judul yang berbeda !" << endl;
                    judulUnik = false;
                    break;
                }
            }
        } while (!judulUnik);
        cout << "Tanggal Peminjaman: ";
        cin >> pinjaman.tanggal_pinjam;
        cout << "Tanggal Pengembalian: ";
        cin >> pinjaman.tanggal_pengembalian;
        antrian.data[antrian.tail] = pinjaman;
        antrian.tail++;

        system("cls");
        cout << "Data telah ditambahkan!!\n\n";
    }
    else
    {
        cout << "Antrian sudah penuh\n";
    }
    getchar();
}

void hapusData(int index)
{
    if (!kosong())
    {
        for (int i = index; i < antrian.tail - 1; i++)
        {
            antrian.data[i] = antrian.data[i + 1];
        }
        antrian.tail--;
    }
    else
    {
        cout << "Data kosong\n";
    }
}

void peminjaman()
{
    const int jumlahBuku = 5;

    // Array untuk menyimpan judul-judul buku
    string judulBuku[jumlahBuku] = {
        "Kalkulus",
        "Pemrograman",
        "Komputer",
        "Linux",
        "Statistika"};

    // Menampilkan daftar judul buku
    cout << "Daftar Judul Buku:" << endl;
    for (int i = 0; i < jumlahBuku; i++)
    {
        cout << i + 1 << ". " << judulBuku[i] << endl;
    }
    tambahData();
}

void pengembalian()
{
    cout << "Pengembalian buku:" << endl;
    string nama, judul;
    cout << "Masukkan Nama: ";
    cin >> nama;
    cout << "Masukkan Judul: ";
    cin >> judul;

    for (int i = antrian.head; i < antrian.tail; i++)
    {
        if (antrian.data[i].nama == nama && antrian.data[i].judul == judul)
        {
            cout << "Data ditemukan:\n";
            cout << "Nama: " << antrian.data[i].nama << endl;
            cout << "NIM: " << antrian.data[i].nim << endl;
            cout << "Judul: " << antrian.data[i].judul << endl;
            cout << "Tanggal Peminjaman: " << antrian.data[i].tanggal_pinjam << endl;
            cout << "Tanggal Pengembalian: " << antrian.data[i].tanggal_pengembalian << endl;

            char konfirmasi;
            cout << "Apakah data ini benar? (y/n): ";
            cin >> konfirmasi;
            if (konfirmasi == 'y' || konfirmasi == 'Y')
            {
                hapusData(i);
                cout << "Buku telah dikembalikan !!" << endl;
            }
            else
            {
                cout << "Pengembalian dibatalkan !!" << endl;
            }
            return;
        }
    }
    cout << "Data peminjaman dengan nama dan judul tersebut tidak ditemukan !!" << endl;
}

void sorting()
{
    for (int i = 0; i < antrian.tail - 1; i++)
    {
        for (int j = 0; j < antrian.tail - i - 1; j++)
        {
            if (antrian.data[j].tanggal_pinjam > antrian.data[j + 1].tanggal_pinjam)
            {
                Buku temp = antrian.data[j];
                antrian.data[j] = antrian.data[j + 1];
                antrian.data[j + 1] = temp;
            }
        }
    }
}

int main()
{
    login();

    int pil;
    do
    {
        tampilData();
        cout << "Menu Utama\n";
        cout << "----------\n";
        cout << "  [1] Peminjaman\n";
        cout << "  [2] Pengembalian\n";
        cout << "  [3] Sorting\n";
        cout << "  [0] Keluar\n";
        cout << "----------\n";
        cout << "Masukan pilihan: ";
        cin >> pil;
        switch (pil)
        {
        case 1:
            peminjaman();
            break;
        case 2:
            pengembalian();
            break;
        case 3:
            sorting();
            break;
        }
    } while (pil != 0);

    return 0;
}
