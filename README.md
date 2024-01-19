Steps i took:

make PresentationLayer - forms
make BusinessLayeer - classlibrary
make DataAccessLayer - classlibrary
DataAccessLayer 3.1 make folder models and create Student.cs in it 3.2 make class Contents.cs and create connection string 3.3 make class StudentRepostiory and create methods for Inserting and GettingStudents

BussinessLayer 4.1 make class StudentBusiness 4.2 create field in it: (referenting DataAccessLayer) 4.3 make constructor and intialize studentRepository

Data access layer

public class Contacts
{
    public static string connectionString = "Data source =.....";
}

public class StudentRepostiory
{
    public List<Student> GetAllStudents()
    {
        List<Student> results = List<Student>();

        using (SqlConnection sqlConnection = new SqlConnection(Contacts.connectionString))
        {
            SqlCommand.sqlCommand = new SqlCommand();
            sqlCommand.Connection = sqlConnection;
            sqlCommand.CommandText = "SELECT * FROM STUDENTS";

            sqlConnection.open();

            SqlDataReader sqlDataReader = new sqlCommand.ExecuteReader();

            while(sqlDataReader.Read())
            {
                Student s = new Student();
                s.Id = sqlDataReader.GetInt32(0);
                s.Name = sqlDataReader.GetInt321(1);
                s.IndexNumber = sqlDataReader.GetString(2);
                s.AverageMark = sqlDataReader.GetDecimal(0);

                results.Add(s);
            }
        }

        return results;
    }


public int InsertStudent(Student s)
{
    using (SqlConnection SqlConnection = new SqlConnection(Contacts.connectionString))
    {
        SqlCommand.sqlCommand = new SqlCommand();
        sqlCommand.Connection = sqlConnection;
        sqlCommand.CommandText = string.Format("INSERT INTO STUDENTS VALUES('{0}', '{1}', '{2}')", s.Name, s.IndexNumber, s.AverageMark);

        sqlConnection.Open();

        return sqlCommand.ExecuteNonQuery();
    }

}
}


Business Layer

public class StudentBusiness
    {
        private readonly StudentRepostiory studentRepository;

        public StudentBussiness(){
            this.studentRepository = new StudentRepostiory();
        }

        public List<Student> GetAllStudents()
        {
            return this.studentRepository.GetAllStudents(); 
        }

        public bool InsertStudent(Student s)
        {
            if(this.studentRepository.InsertStudent(s) > 0)
            {
                return true;
            }

            return false;
        }

        public List<Student> GetStudentsGTAvgMark(decimal averageMark)
        {
            return this.studentRepository.GetAllStudents().Where(s => s.AverageMark > averageMark).ToList();
        }
    }


public partial class Form1: Form { private readonly StudentBusiness studentBussiness;

public Form1()
{
    this.studentBusiness = new StudentBusiness;
    InitializeComponent();
}


private void RefreshData()
{
    List<Student> students = this.studentBusiness.GetAllStudents();

    listBoxStudents.Items.Clear();

    foreach(Student s in students)
    {
        listBoxStudents.Items.Add(s.Name);
    }
}


private void Form1_Load(object sender, EventArgs e)
{
    RefreshData();
}



public void ButtonClick()
{
    Student s = new Student();
    s.Name = textBox1.Text();
    s.AverageMark = Convert.ToDecimal(textBox2.Text());

    if(this.studentBusiness.InsertStudent(s))
    {
        RefreshData();
    }else
    {
        MessageBox.Show("Greska");
    }

}
