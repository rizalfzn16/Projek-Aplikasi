using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Formmasuk
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }   


        private void button1_Click_1(object sender, EventArgs e)
        {
            if (textBox1.Text == "Rizalfauzan16" && textBox2.Text == "Rizal1234")
            {
                new Formmenu().Show();
                this.Hide();
            }

            else
            {
                MessageBox.Show("The Username and Password you entered is Inccorect, Try Again!");
                textBox1.Clear();
                textBox2.Clear();
                textBox1.Focus();

            }
        }

        private void label1_Click_1(object sender, EventArgs e)
        {
            textBox1.Clear();
            textBox2.Clear();
            textBox1.Focus();
        }

        private void label2_Click_1(object sender, EventArgs e)
        {
            Application.Exit();
        }
    }
}


using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

using MySql.Data.MySqlClient;

namespace Formmasuk
{

    public partial class Formmenu : Form
    {
        MySqlConnection koneksi = new MySqlConnection("Database=permainan;server=localhost;uid=root;password=;");

        public Formmenu()
        {
            InitializeComponent();
        }

        private void Formmenu_Load(object sender, EventArgs e)
        {
            koneksi.Open();
            LoadData();
            koneksi.Close();
        }

        internal static void open()
        {
            throw new NotImplementedException();
        }

        public void LoadData()
        {
            //inisialisasi mysqlcommand//
            MySqlCommand command;
            command = koneksi.CreateCommand();

            //ambil data dari permainan query//
            command.CommandText = "SELECT * FROM pendaftaran";

            //masukan data ke datagrid//
            MySqlDataAdapter adapter = new MySqlDataAdapter(command);
            DataSet dataset = new DataSet();
            adapter.Fill(dataset);
            dataGridView1.DataSource = dataset.Tables[0].DefaultView;

        }

        private void button1_Click(object sender, EventArgs e)
        {

            //buka koneksi//
            koneksi.Open();

            //inisialisasi Mysqlcommand//
            MySqlCommand command;
            command = koneksi.CreateCommand();

            //input data query//
            command.CommandText = "insert into pendaftaran(id_ktp, Nama, Alamat, No_Antrian) VALUES (@id_ktp, @Nama, @Alamat, @No_Antrian);";


            //tambahkan parameters//
            command.Parameters.AddWithValue("@id_ktp", textBox1.Text);
            command.Parameters.AddWithValue("@Nama", textBox3.Text);
            command.Parameters.AddWithValue("@Alamat", textBox4.Text);
            command.Parameters.AddWithValue("@No_Antrian", textBox2.Text);

            //munculkan massegebox berhasil//
            MessageBox.Show("Data berhasil diinput");

            //eksekusi query//
            command.ExecuteNonQuery();

            //kosongkan form//
            textBox1.Text = "";
            textBox3.Text = "";
            textBox4.Text = "";
            textBox2.Text = "";

            //load data ke datagrid//
            LoadData();

            //tutup koneksi//
            koneksi.Close();
        }

        private void button2_Click(object sender, EventArgs e)
        {
            new PrintStruk().Show();
            this.Hide();
        }
    }
}

