### ThinkPHP5.0 安装使用Doctrine ORM
#### composer安装
    "require": {
            "doctrine/orm": "^2.6.2"
        }
> composer install //安装doctrine ORM

#### 初始化Doctrine配置
- Doctrine有一个命令行界面，允许您访问SchemaTool，这是一个可以完全基于定义的实体类及其元数据生成关系数据库模式的组件。要使此工具起作用，cli-config.php项目根目录中必须存在一个文件:

        //项目的根目录创建 cli-config.php
        <?php
        
        use Doctrine\ORM\Tools\Setup;
        use Doctrine\ORM\EntityManager;
        
        require_once 'vendor/autoload.php';
        $config = require_once 'application/database.php';
        $isDevMoe = true;
        $configuration =  Setup::createAnnotationMetadataConfiguration(array(__DIR__ . '/application/entity'), $isDevMoe);
        $conn = array(
            'driver'   => 'pdo_mysql',
            'user'     => $config['username'] ? $config['username'] : 'root',
            'password' => $config['password'] ? $config['password'] : 'root',
            'dbname'   => $config['database'] ? $config['database'] : 'exp',
            'port' => $config['hostport'] ? $config['hostport'] : 3306,
            'charset' => 'utf8'
        );
        $entityManager = EntityManager::create($conn, $configuration);
        return \Doctrine\ORM\Tools\Console\ConsoleRunner::createHelperSet($entityManager);
#### Entity.php
    //application/Entity.php
    <?php
    
    namespace app;
    
    use think\Config;
    use Doctrine\ORM\Tools\Setup;
    use Doctrine\ORM\EntityManager;
    
    class Entity
    {
        public static function getEntityManager()
        {
            $isDevMoe = true;
            $config =  Setup::createAnnotationMetadataConfiguration(array(APP_PATH . '/entity'), $isDevMoe);
            $conn = array(
                'driver'   => 'pdo_mysql',
                'user'     => Config::get('database.username') ? Config::get('database.username') : 'root',
                'password' => Config::get('database.password') ? Config::get('database.password') : 'root',
                'dbname'   => Config::get('database.database') ? Config::get('database.database') : 'exp',
                'port' => Config::get('database.hostport') ? Config::get('database.hostport') : 3306,
                'charset' => 'utf8'
            );
            return EntityManager::create($conn, $config);
        }
    }
#### 定义Entity信息
    //application/entity/User.php
    <?php
    
    namespace app\entity;
    
    /**
     * @Entity
     * @Table(name="User",options={"comment":"用户表"})
     */
    class User{
    
        /**
         * @var int
         *
         * @Id
         * @Column(type="integer")
         * @GeneratedValue
         */
        protected $id;
    
        /**
         * @var string
         *
         * @Column(name="username", type="string", length=32, nullable=true, options={"comment": "账号"})
         */
        protected $username;
    
        public function setId($id)
        {
            $this->id = $id;
        }
    
        public function getId()
        {
            return $this->id;
        }
    
        public function setUsername($username)
        {
            $this->username = $username;
        }
    
        public function getUsername()
        {
            return $this->username;
        }
    }
    
#### 命令生成/更新 数据表(不需要操作数据库,自动生成/更新User表(字段))
> vendor/bin/doctrine orm:schema-tool:update --force

#### 补充扩展
- 具体参考[Doctrine手册](https://www.doctrine-project.org/projects/doctrine-orm/en/2.6/index.html)
- 命令难记的话可以使用 Tp5 command 自定义命令
- 暂时想不到了。。。
