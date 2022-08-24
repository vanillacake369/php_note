# preparedStatement쿼리 시 bind_param()이 bool이 뜨는 에러는 쿼리문이 잘못된 경우!

I'm busy on a function that gets settings from a DB, and suddenly, I ran into this error:

```
Fatal error: Call to a member function bind_param() on boolean in C:\xampp2\htdocs\application\classes\class.functions.php on line 16

```

Normally, this would mean that I'm selecting stuff from unexisting tables and stuff. But in this case, I 'm not...

Here's the `getSetting` function:

```
public function getSetting($setting)
{
    $query = $this->db->conn->prepare('SELECT value, param FROM ws_settings WHERE name = ?');
    $query->bind_param('s', $setting);
    $query->execute();
    $query->bind_result($value, $param);
    $query->store_result();
    if ($query->num_rows() > 0)
    {
        while ($query->fetch())
        {
            return $value;
            if ($param === '1')
            {
                $this->tpl->createParameter($setting, $value);
            }
        }
    }
    else
    {
        __('invalid.setting.request', $setting);
    }
}

```

The `$this->db` variable is passed through a constructor. In case of need, here is it:

```
public function __construct($db, $data, $tpl)
{
    $this->db = $db;
    $this->tpl = $tpl;
    $this->data = $data;
    $this->data->setData('global', 'theme', $this->getSetting('theme'));
}

```

Also, since I'm making use of a database, my database connection:

```
class Database
{
    private $data;

    public function __construct($data)
    {
    $this->data = $data;
    $this->conn = new MySQLi(
      $this->data->getData('database', 'hostname'),
      $this->data->getData('database', 'username'),
      $this->data->getData('database', 'password'),
      $this->data->getData('database', 'database')
    );
    if ($this->conn->errno)
    {
        __('failed.db.connection', $this->conn->errno);
    }
    date_default_timezone_set('Europe/Amsterdam');
}

```

I've already tested the connection, 100% positive that it works as intended. I'm setting the DB connection things in a configuration file:

```
'database' => array(
    'hostname' => '127.0.0.1',
    'username' => 'root',
    'password' => ******,
    'database' => 'wscript'
)

```

Now the weird thing is; the table exists, the requested setting exists, the DB exists, but still, that error won't leave. Here's some proof that the DB is correct:

![preparedStatement%E1%84%8F%E1%85%AF%E1%84%85%E1%85%B5%20%E1%84%89%E1%85%B5%20bind_param()%E1%84%8B%E1%85%B5%20bool%E1%84%8B%E1%85%B5%20%E1%84%84%E1%85%B3%E1%84%82%206cb060fd63344a3192f163b6f3c9a505/6YSL1.png](preparedStatement%E1%84%8F%E1%85%AF%E1%84%85%E1%85%B5%20%E1%84%89%E1%85%B5%20bind_param()%E1%84%8B%E1%85%B5%20bool%E1%84%8B%E1%85%B5%20%E1%84%84%E1%85%B3%E1%84%82%206cb060fd63344a3192f163b6f3c9a505/6YSL1.png)