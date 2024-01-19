Koraci:
1. napraviti PresentationLayer - web forms
2. napraviti BusinessLayeer - classlibrary
3. napraviti DataAccessLayer - classlibrary


3. DataAccessLayer 
3.1 Napraviti folder models i u njemu kreirati Student.cs
3.2 napraviti klasu Contents.cs i u njega staviti connection string
3.3 napraviti klasu StudentRepostiory i u njemu kreirati funkcije za kreiranje i vracanje podataka studenta

4. BussinessLayer
4.1 Napraviti klasu StudentBusiness
4.2 u StudentBusiness napraviti polje:  (napraviti referencu ka DataAccessLayer)
4.3 Napraviti konstruktor i inicijalizovati studentRepository


//Data access layer 

    public class Contacts
    {
        public static string connectionString = "Data source =.....";
    }


    public class StudentRepostiory
    {
    public List<Student> GetAllStudents()
    {
        List<Student> results = List<Student>();
    
        using (SqlConnection SqlConnection = new SqlConnection(Contacts.conneectionString))
        {
            SqlCommand.sqlCommand = new SqlCommand();
            sqlCommand.Connection = sqlConnection;
            sqlCommand.CommandText = 'SELECT * FROM STUDENTS";
    
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
        using (SqlConnection SqlConnection = new SqlConnection(Contacts.conneectionString))
        {
            SqlCommand.sqlCommand = new SqlCommand();
            sqlCommand.Connection = sqlConnection;
            sqlCommand.CommandText = string.Format("INSERT INTO STUDENTS VALUES('{0}', '{1}', '{2}')", s.Name, s.IndexNumber, s.AverageMark);
    
            sqlConnection.Open();
    
            return sqlCommand.ExecuteNonQuery();
        }
    
    }
    }

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
    




//PresentationLayer

1.1 Za prikazivanje studentats se koristi ListBox


    public partial class Form1: Form
    {
        private readonly StudentBusiness studentBussiness;

    public Form1()
    {
        this.studentBusiness = new StudentBusiness;
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

}
