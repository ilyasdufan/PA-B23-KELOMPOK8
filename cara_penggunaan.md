Cara Penggunaanya

def connect_database():
    try:
        conn = mysql.connector.connect(
            host="localhost",   
            user="root",
            password="",
            database="ragam_hayati"
        )
        print("Koneksi ke database berhasil!")
        return conn
    except mysql.connector.Error as err:
        print("Error:", err)                                
        return None
# Kelas untuk entitas Satwa
class Satwa:
    def __init__(self, conn):
        self.conn = conn

    # Method untuk menampilkan semua data satwa
    def show_data(self):
        cursor = self.conn.cursor()
        cursor.execute("SELECT * FROM satwa")
        result = cursor.fetchall()

        if result:
            # Buat objek PrettyTable
            table = PrettyTable()
            table.field_names = ["ID", "Nama Satwa", "Ordo", "id kawasan", "status perlindungan"]

            # Tambahkan baris ke PrettyTable
            for row in result:
                table.add_row([row[0], row[1], row[2], row[3], row [4]])

            # Cetak PrettyTable
            print(table)
        else:
            print("Tidak ada data tentang satwa yang ditemukan.")

    # Method untuk menambahkan data satwa baru
    def add_data(self, id_satwa , nama_satwa, ordo, id_kawasan, status_perlindungan):
        cursor = self.conn.cursor()
        sql = "INSERT INTO satwa (id_satwa, nama_satwa, ordo, id_kawasan, status_perlindungan) VALUES (%s, %s, %s, %s,%s)"
        val = (id_satwa, nama_satwa, ordo, id_kawasan, status_perlindungan)
        cursor.execute(sql, val)
        self.conn.commit()
        print("Data satwa berhasil ditambahkan.")

    # Method untuk menghapus data satwa berdasarkan ID
    def delete_data(self, id_satwa):
        cursor = self.conn.cursor()
        sql = "DELETE FROM satwa WHERE id_satwa = %s"
        val = (id_satwa,)
        cursor.execute(sql, val)
        self.conn.commit()
        print("Data satwa berhasil dihapus.")

    def search_by_name(self, keyword):
        cursor = self.conn.cursor()
        sql = "SELECT * FROM satwa WHERE nama_satwa LIKE %s"
        cursor.execute(sql, ("%" + keyword + "%",))
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID", "Nama Satwa", "Ordo", "ID Kawasan", "Status Perlindungan"]

            for row in result:
                table.add_row(row)

            print(table)
        else:
            print("Tidak ada satwa yang sesuai dengan kata kunci pencarian.")

    def search_by_id(self, id_satwa):
        cursor = self.conn.cursor()
        sql = "SELECT * FROM satwa WHERE id_satwa = %s"
        cursor.execute(sql, (id_satwa,))
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID", "Nama Satwa", "Ordo", "ID Kawasan", "Status Perlindungan"]
            table.add_row(result[0])
            print(table)
        else:
            print("Satwa dengan ID tersebut tidak ditemukan.")

    def update_data(self, id_satwa, nama_satwa, ordo, id_kawasan, status_perlindungan):
        cursor = self.conn.cursor()
        sql = "UPDATE satwa SET  nama_satwa = %s, ordo = %s, id_kawasan = %s, status_perlindungan = %s WHERE id_satwa = %s"
        val = ( nama_satwa, ordo, id_kawasan, status_perlindungan, id_satwa)
        cursor.execute(sql, val)
        self.conn.commit()
        print("Data satwa berhasil diperbarui.")


# Kelas untuk entitas Linked List
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        last_node = self.head
        while last_node.next:
            last_node = last_node.next
        last_node.next = new_node

    def display(self):
        current_node = self.head
        while current_node:
            print(current_node.data)
            current_node = current_node.next


# Kelas untuk entitas Kawasan Konservasi
class KawasanKonservasi:
    def __init__(self, conn):
        self.conn = conn
        self.linked_list = LinkedList()

    def show_data(self):
        cursor = self.conn.cursor()
        cursor.execute("SELECT * FROM kawasan_konservasi")
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID Kawasan", "Nama Kawasan", "ID Admin", "Status Kawasan"]

            for row in result:
                table.add_row(row)

            print(table)
        else:
            print("Tidak ada data tentang kawasan konservasi yang ditemukan.")

    def display_linked_list(self):
        self.linked_list.display()

    def add_data(self, id_kawasan, nama_kawasan, id_admin, status_kawasan):
        cursor = self.conn.cursor()
        sql = "INSERT INTO kawasan_konservasi (id_kawasan, nama_kawasan, id_admin, status_kawasan) VALUES (%s, %s, %s, %s)"
        val = (id_kawasan, nama_kawasan, id_admin, status_kawasan)
        cursor.execute(sql, val)
        self.conn.commit()
        print("Data kawasan konservasi berhasil ditambahkan.")

    def delete_data(self, id_kawasan):
        cursor = self.conn.cursor()
        sql = "DELETE FROM kawasan_konservasi WHERE id_kawasan = %s"
        val = (id_kawasan,)
        cursor.execute(sql, val)
        self.conn.commit()
        print("Data kawasan konservasi berhasil dihapus.")


# Kelas untuk entitas Berita
class Berita:
    def __init__(self, conn):
        self.conn = conn

    def show_data(self):
        cursor = self.conn.cursor()
        cursor.execute("SELECT * FROM berita")
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID Berita", "Judul Berita", "ID Admin", "Tanggal Terbit"]

            for row in result:
                table.add_row(row)

            print(table)
        else:
            print("Tidak ada data tentang berita yang ditemukan.")

    def show_data_ascending(self):
        cursor = self.conn.cursor()
        cursor.execute("SELECT * FROM berita ORDER BY tanggal_terbit ASC")
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID Berita", "Judul Berita", "ID Admin", "Tanggal Terbit"]

            for row in result:
                table.add_row(row)

            print(table)
        else:
            print("Tidak ada data tentang berita yang ditemukan.")

    def show_data_descending(self):
        cursor = self.conn.cursor()
        cursor.execute("SELECT * FROM berita ORDER BY tanggal_terbit DESC")
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID Berita", "Judul Berita", "ID Admin", "Tanggal Terbit"]

            for row in result:
                table.add_row(row)

            print(table)
        else:
            print("Tidak ada data tentang berita yang ditemukan.")

    def add_data(self, judul_berita, id_admin, tanggal_terbit):
        cursor = self.conn.cursor()
        sql = "INSERT INTO berita (judul_berita, id_admin, tanggal_terbit) VALUES (%s, %s, %s)"
        val = (judul_berita, id_admin, tanggal_terbit)
        cursor.execute(sql, val)
        self.conn.commit()
        print("Berita berhasil ditambahkan.")

    def delete_data(self, id_berita):
        cursor = self.conn.cursor()
        sql = "DELETE FROM berita WHERE id_berita = %s"
        val = (id_berita,)
        cursor.execute(sql, val)
        self.conn.commit()
        print("Berita berhasil dihapus.")

    def search_by_title(self, keyword):
        cursor = self.conn.cursor()
        sql = "SELECT * FROM berita WHERE judul_berita LIKE %s"
        cursor.execute(sql, ("%" + keyword + "%",))
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID Berita", "Judul Berita", "ID Admin", "Tanggal Terbit"]

            for row in result:
                table.add_row(row)

            print(table)
        else:
            print("Tidak ada berita yang sesuai dengan kata kunci pencarian.")

    def search_by_id(self, id_berita):
        cursor = self.conn.cursor()
        sql = "SELECT * FROM berita WHERE id_berita = %s"
        cursor.execute(sql, (id_berita,))
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID Berita", "Judul Berita", "ID Admin", "Tanggal Terbit"]
            table.add_row(result[0])
            print(table)
        else:
            print("Berita dengan ID tersebut tidak ditemukan.")

    def update_data(self, id_berita, judul_berita, id_admin, tanggal_terbit):
        cursor = self.conn.cursor()
        sql = "UPDATE berita SET judul_berita = %s, id_admin = %s, tanggal_terbit = %s WHERE id_berita = %s"
        val = (judul_berita, id_admin, tanggal_terbit, id_berita)
        cursor.execute(sql, val)
        self.conn.commit()
        print("Data berita berhasil diperbarui.")


# Fungsi untuk menampilkan menu utama
def main_menu(conn):
    while True:
        print("\nMenu Utama:")
        print("1. Satwa")
        print("2. Kawasan Konservasi")
        print("3. Berita")
        print("0. Keluar")
        choice = input("Masukkan pilihan: ")

        if choice == "1":
            satwa_menu(conn)
        elif choice == "2":
            kawasan_konservasi_menu(conn)
        elif choice == "3":
            berita_menu(conn)
        elif choice == "0":
            print("Program selesai.")
            break
        else:
            print("Pilihan tidak valid. Silakan coba lagi.")


# Fungsi untuk menampilkan menu untuk entitas Satwa
def satwa_menu(conn):
    satwa = Satwa(conn)
    while True:
        print("\nMenu Satwa:")
        print("1. Tampilkan Data Satwa")
        print("2. Tambah Data Satwa")
        print("3. Hapus Data Satwa")
        print("4. Cari Satwa")
        print("5. Update Data Satwa")
        print("0. Kembali ke Menu Utama")
        choice = input("Masukkan pilihan: ")

        if choice == "1":
            satwa.show_data()
        elif choice == "2":
            id_satwa = input("masukan id satwa :")
            nama_satwa = input("Masukkan nama satwa baru: ")
            ordo = input("Masukkan ordo satwa: ")
            id_kawasan = input("Masukkan ID kawasan: ")
            status_perlindungan = input("Masukkan status perlindungan: ")
            satwa.add_data(id_satwa, nama_satwa, ordo, id_kawasan, status_perlindungan)
        elif choice == "3":
            id_satwa = input("Masukkan ID satwa yang ingin dihapus: ")
            satwa.delete_data(id_satwa)
        elif choice == "4":
            search_option = input("Pilih opsi pencarian (1: Berdasarkan Nama, 2: Berdasarkan ID): ")
            if search_option == "1":
                keyword = input("Masukkan kata kunci pencarian (nama satwa): ")
                satwa.search_by_name(keyword)
            elif search_option == "2":
                id_satwa = input("Masukkan ID satwa: ")
                satwa.search_by_id(id_satwa)
            else:
                print("Opsi pencarian tidak valid.")
        elif choice == "5":
            id_satwa = input("Masukkan ID satwa: ")
            nama_satwa = input("Masukkan nama satwa baru: ")
            ordo = input("Masukkan ordo satwa: ")
            id_kawasan = input("Masukkan ID kawasan: ")
            status_perlindungan = input("Masukkan status perlindungan: ")
            satwa.update_data(id_satwa, nama_satwa, ordo, id_kawasan, status_perlindungan)
        elif choice == "0":
            break
        else:
            print("Pilihan tidak valid. Silakan coba lagi.")


# Fungsi untuk menampilkan menu untuk entitas kawasan_konservasi
def kawasan_konservasi_menu(conn):
    kawasan_konservasi = KawasanKonservasi(conn)
    while True:
        print("\nMenu Kawasan Konservasi:")
        print("1. Tampilkan Data Kawasan Konservasi")
        print("2. Tambah Data Kawasan Konservasi")
        print("3. Hapus Data Kawasan Konservasi")
        print("0. Kembali ke Menu Utama")
        choice = input("Masukkan pilihan: ")

        if choice == "1":
            kawasan_konservasi.show_data()
        elif choice == "2":
            id_kawasan = input("Masukkan ID kawasan: ")
            nama_kawasan = input("Masukkan nama kawasan baru: ")
            id_admin = input("Masukkan ID admin: ")
            status_kawasan = input("Masukkan status kawasan: ")
            kawasan_konservasi.add_data(id_kawasan, nama_kawasan, id_admin, status_kawasan)
        elif choice == "3":
            id_kawasan = input("Masukkan ID kawasan yang ingin dihapus: ")
            kawasan_konservasi.delete_data(id_kawasan)
        elif choice == "0":
            break
        else:
            print("Pilihan tidak valid. Silakan coba lagi.")


# Fungsi untuk menampilkan menu untuk entitas berita
def berita_menu(conn):
    berita = Berita(conn)
    while True:
        print("\nMenu Berita:")
        print("1. Tampilkan Data Berita (Ascending)")
        print("2. Tampilkan Data Berita (Descending)")
        print("3. Tambah Data Berita")
        print("4. Hapus Data Berita")
        print("5. Cari Berita")
        print("6. Update Data Berita")
        print("0. Kembali ke Menu Utama")
        choice = input("Masukkan pilihan: ")

        if choice == "1":
            berita.show_data_ascending()
        elif choice == "2":
            berita.show_data_descending()
        elif choice == "3":
            judul_berita = input("Masukkan judul berita baru: ")
            id_admin = input("Masukkan ID admin: ")
            tanggal_terbit = input("Masukkan tanggal terbit (YYYY-MM-DD): ")
            berita.add_data(judul_berita, id_admin, tanggal_terbit)
        elif choice == "4":
            id_berita = input("Masukkan ID berita yang ingin dihapus: ")
            berita.delete_data(id_berita)
        elif choice == "5":
            search_option = input("Pilih opsi pencarian (1: Berdasarkan Judul, 2: Berdasarkan ID): ")
            if search_option == "1":
                keyword = input("Masukkan kata kunci pencarian (judul berita): ")
                berita.search_by_title(keyword)
            elif search_option == "2":
                id_berita = input("Masukkan ID berita: ")
                berita.search_by_id(id_berita)
            else:
                print("Opsi pencarian tidak valid.")
        elif choice == "6":
            id_berita = input("Masukkan ID berita yang ingin diperbarui: ")
            judul_berita = input("Masukkan judul berita baru: ")
            id_admin = input("Masukkan ID admin: ")
            tanggal_terbit = input("Masukkan tanggal terbit (YYYY-MM-DD): ")
            berita.update_data(id_berita, judul_berita, id_admin, tanggal_terbit)
        elif choice == "0":
            break
        else:
            print("Pilihan tidak valid. Silakan coba lagi.")


class Admin:
    def __init__(self, conn):
        self.conn = conn

    def login(self):
        while True:
            nama_admin = input("Masukkan nama admin: ")
            password = input("Masukkan password: ")

            cursor = self.conn.cursor()
            sql = "SELECT * FROM admin WHERE nama_admin = %s AND password = %s"
            val = (nama_admin, password)
            cursor.execute(sql, val)
            result = cursor.fetchone()

            if result:
                print("Login berhasil sebagai admin.")
                return True
            else:
                print("Login gagal. Nama admin atau password salah.")
                coba_lagi = input("Coba lagi? (y/n): ")
                if coba_lagi.lower() != 'y':
                    return False

    def menu_admin(self):
        while True:
            print("\nMenu Admin:")
            print("1. Kelola Data Satwa")
            print("2. Kelola Data Kawasan Konservasi")
            print("3. Kelola Data Berita")
            print("0. Keluar")
            choice = input("Masukkan pilihan: ")

            if choice == "1":
                self.kelola_satwa()
            elif choice == "2":
                self.kelola_kawasan_konservasi()
            elif choice == "3":
                self.kelola_berita()
            elif choice == "0":
                print("Keluar dari menu admin.")
                break
            else:
                print("Pilihan tidak valid. Silakan coba lagi.")

    def kelola_satwa(self):
        satwa = Satwa(self.conn)
        while True:
            print("\nMenu Satwa:")
            print("1. Tampilkan Data Satwa")
            print("2. Tambah Data Satwa")
            print("3. Hapus Data Satwa")
            print("4. Cari Satwa")
            print("5. Update Data Satwa")
            print("0. Kembali ke Menu admin")
            choice = input("Masukkan pilihan: ")

            if choice == "1":
                satwa.show_data()
            elif choice == "2":
                id_satwa = input("masukan id satwa :")
                nama_satwa = input("Masukkan nama satwa baru: ")
                ordo = input("Masukkan ordo satwa: ")
                id_kawasan = input("Masukkan ID kawasan: ")
                status_perlindungan = input("Masukkan status perlindungan: ")
                satwa.add_data(id_satwa, nama_satwa, ordo, id_kawasan, status_perlindungan)
            elif choice == "3":
                id_satwa = input("Masukkan ID satwa yang ingin dihapus: ")
                satwa.delete_data(id_satwa)
            elif choice == "4":
                search_option = input("Pilih opsi pencarian (1: Berdasarkan Nama, 2: Berdasarkan ID): ")
                if search_option == "1":
                    keyword = input("Masukkan kata kunci pencarian (nama satwa): ")
                    satwa.search_by_name(keyword)
                elif search_option == "2":
                    id_satwa = input("Masukkan ID satwa: ")
                    satwa.search_by_id(id_satwa)
                else:
                    print("Opsi pencarian tidak valid.")
            elif choice == "5":
                id_satwa = input("Masukkan ID satwa: ")
                nama_satwa = input("Masukkan nama satwa baru: ")
                ordo = input("Masukkan ordo satwa: ")
                id_kawasan = input("Masukkan ID kawasan: ")
                status_perlindungan = input("Masukkan status perlindungan: ")
                result = satwa.update_data(id_satwa, nama_satwa, ordo, id_kawasan, status_perlindungan)
                if not result:
                    print("Nama atau ID satwa tidak ditemukan.")
            elif choice == "0":
                break
            else:
                print("Pilihan tidak valid. Silakan coba lagi.")

    def kelola_kawasan_konservasi(self):
        kawasan_konservasi = KawasanKonservasi(self.conn)
        while True:
            print("\nMenu Kawasan Konservasi:")
            print("1. Tampilkan Data Kawasan Konservasi")
            print("2. Tambah Data Kawasan Konservasi")
            print("3. Hapus Data Kawasan Konservasi")
            print("0. Kembali ke Menu Admin")
            choice = input("Masukkan pilihan: ")

            if choice == "1":
                kawasan_konservasi.show_data()
            elif choice == "2":
                id_kawasan = input("Masukkan ID kawasan: ")
                nama_kawasan = input("Masukkan nama kawasan baru: ")
                id_admin = input("Masukkan ID admin: ")
                status_kawasan = input("Masukkan status kawasan: ")
                kawasan_konservasi.add_data(id_kawasan, nama_kawasan, id_admin, status_kawasan)
            elif choice == "3":
                id_kawasan = input("Masukkan ID kawasan yang ingin dihapus: ")
                kawasan_konservasi.delete_data(id_kawasan)
            elif choice == "0":
                print("Kembali ke menu admin.")
                break
            else:
                print("Pilihan tidak valid. Silakan coba lagi.")
        
    def kelola_berita(self):
        berita = Berita(self.conn)
        while True:
            print("\nMenu Berita:")
            print("1. Tampilkan Data Berita (Ascending)")
            print("2. Tampilkan Data Berita (Descending)")
            print("3. Tambah Data Berita")
            print("4. Hapus Data Berita")
            print("5. Cari Berita")
            print("6. Update Data Berita")
            print("0. Kembali ke Menu Admin")
            choice = input("Masukkan pilihan: ")

            if choice == "1":
                berita.show_data_ascending()
            elif choice == "2":
                berita.show_data_descending()
            elif choice == "3":
                judul_berita = input("Masukkan judul berita baru: ")
                id_admin = input("Masukkan ID admin: ")
                tanggal_terbit = input("Masukkan tanggal terbit (YYYY-MM-DD): ")
                berita.add_data(judul_berita, id_admin, tanggal_terbit)
            elif choice == "4":
                id_berita = input("Masukkan ID berita yang ingin dihapus: ")
                berita.delete_data(id_berita)
            elif choice == "5":
                search_option = input("Pilih opsi pencarian (1: Berdasarkan Judul, 2: Berdasarkan ID): ")
                if search_option == "1":
                    keyword = input("Masukkan kata kunci pencarian (judul berita): ")
                    berita.search_by_title(keyword)
                elif search_option == "2":
                    id_berita = input("Masukkan ID berita: ")
                    berita.search_by_id(id_berita)
                else:
                    print("Opsi pencarian tidak valid.")
            elif choice == "6":
                id_berita = input("Masukkan ID berita yang ingin diperbarui: ")
                judul_berita = input("Masukkan judul berita baru: ")
                id_admin = input("Masukkan ID admin: ")
                tanggal_terbit = input("Masukkan tanggal terbit (YYYY-MM-DD): ")
                berita.update_data(id_berita, judul_berita, id_admin, tanggal_terbit)
            elif choice == "0":
                print("Kembali ke menu admin.")
                break
            else:
                print("Pilihan tidak valid. Silakan coba lagi.")

class User:
    def __init__(self, conn):
        self.conn = conn

    def login(self):
        nama_user = input("Masukkan nama user: ")
        password = input("Masukkan password: ")

        cursor = self.conn.cursor()
        sql = "SELECT * FROM user WHERE nama_user = %s AND password = %s"
        val = (nama_user, password)
        cursor.execute(sql, val)
        result = cursor.fetchall()  # Menggunakan fetchall() untuk mengambil semua baris yang sesuai

        if result:
            print("Login berhasil sebagai user.")
            for row in result:
                print(row)  # Anda dapat melakukan operasi apa pun dengan baris-baris yang sesuai
            return True
        else:
            print("Login gagal. Nama user atau password salah.")
            return False


    def menu_user(self):
        while True:
            print("\nMenu User:")
            print("1. Tampilkan Data Satwa")
            print("2. Cari Satwa Berdasarkan Nama")
            print("3. Tampilkan Data Berita (Terlama)")
            print("4. Tampilkan Data Berita (Terbaru)")
            print("5. Tampilkan Data Kawasan Konservasi")
            print("0. Keluar")
            choice = input("Masukkan pilihan: ")

            if choice == "1":
                self.tampilkan_data_satwa()
            elif choice == "2":
                self.cari_satwa_berdasarkan_nama()
            elif choice == "3":
                self.tampilkan_data_berita_ascending()
            elif choice == "4":
                self.tampilkan_data_berita_descending()
            elif choice == "5":
                self.tampilkan_data_kawasan_konservasi()
            elif choice == "0":
                print("Keluar dari menu user.")
                break
            else:
                print("Pilihan tidak valid. Silakan coba lagi.")

    def tampilkan_data_satwa(self):
        cursor = self.conn.cursor()
        cursor.execute("SELECT * FROM satwa")
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID", "Nama Satwa", "Ordo", "ID Kawasan", "Status Perlindungan"]
            for row in result:
                table.add_row(row)
            print(table)
        else:
            print("Tidak ada data satwa yang ditemukan.")

    def cari_satwa_berdasarkan_nama(self):
        keyword = input("Masukkan kata kunci nama satwa: ")
        cursor = self.conn.cursor()
        sql = "SELECT * FROM satwa WHERE nama_satwa LIKE %s"
        cursor.execute(sql, ("%" + keyword + "%",))
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID", "Nama Satwa", "Ordo", "ID Kawasan", "Status Perlindungan"]
            for row in result:
                table.add_row(row)
            print(table)
        else:
            print("Tidak ada satwa yang sesuai dengan kata kunci pencarian.")

    def tampilkan_data_berita_ascending(self):
        cursor = self.conn.cursor()
        cursor.execute("SELECT * FROM berita ORDER BY tanggal_terbit ASC")
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID Berita", "Judul Berita", "ID Admin", "Tanggal Terbit"]
            for row in result:
                table.add_row(row)
            print(table)
        else:
            print("Tidak ada data berita yang ditemukan.")
    
    def tampilkan_data_berita_descending(self):
        cursor = self.conn.cursor()
        cursor.execute("SELECT * FROM berita ORDER BY tanggal_terbit DESC")
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID Berita", "Judul Berita", "ID Admin", "Tanggal Terbit"]
            for row in result:
                table.add_row(row)
            print(table)
        else:
            print("Tidak ada data berita yang ditemukan.")

    def tampilkan_data_kawasan_konservasi(self):
        cursor = self.conn.cursor()
        cursor.execute("SELECT * FROM kawasan_konservasi")
        result = cursor.fetchall()

        if result:
            table = PrettyTable()
            table.field_names = ["ID Kawasan", "Nama Kawasan", "ID Admin", "Status Kawasan"]
            for row in result:
                table.add_row(row)
            print(table)
        else:
            print("Tidak ada data kawasan konservasi yang ditemukan.")

def main_menu(conn):
    while True:
        print("\nMenu Utama:")
        print("1. Admin")
        print("2. User")
        print("0. Keluar")
        choice = input("Masukkan pilihan: ")

        if choice == "1":
            admin = Admin(conn)
            if admin.login():
                admin.menu_admin()
        elif choice == "2":
            user = User(conn)
            if user.login():
                user.menu_user()
        elif choice == "0":
            print("Program selesai.")
            break
        else:
            print("Pilihan tidak valid. Silakan coba lagi.")

def main():
    connection = connect_database()
    if connection:
        main_menu(connection)
        connection.close()
    else:
        print("Koneksi ke database gagal. Program berhenti.")

if __name__ == "__main__":
    main()
