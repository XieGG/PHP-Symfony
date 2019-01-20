### Fixures相关

相关文档
> https://symfony.com/doc/current/bundles/DoctrineFixturesBundle/index.html#multiple-files

安装
>$ composer require --dev doctrine/doctrine-fixtures-bundle

加载
>$ php bin/console doctrine:fixtures:load

services.yml 服务

	system.att_set_datafixtures:
	    class: SystemBundle\DataFixtures\AttachmentFixtures
	    tags: ['doctrine.fixture.orm']
一个完整的Fixures

	<?php
	namespace SystemBundle\DataFixtures;
	
	use SystemBundle\Entity\Attachment; 
	use Doctrine\Bundle\FixturesBundle\Fixture;
	use Doctrine\Common\Persistence\ObjectManager;
	
	class AttachmentFixtures extends Fixture
	{
	    public function load(ObjectManager $manager)
	    {
	        // TODO: Implement load() method.
	        $result = $this->truncateTable($manager);
	        if ($result['code'] == 0){
	            echo  $result['msg'];
	            return false;
	        }
	        foreach($this->data() as $val){
	            $att = new Attachment();
	            $att->setGid($val['gid']);
	            $att->setContent($val['content']);
	            $manager->persist($att);
	            $manager->flush();
	        }
	    }
	
	    private function truncateTable($em)
	    {
	        $classMetaData = $em->getClassMetadata(Attachment::class);
	        $connection = $em->getConnection();
	        $dbPlatform = $connection->getDatabasePlatform();
	        $connection->beginTransaction();
	        try {
	            $connection->query('SET FOREIGN_KEY_CHECKS=0');
	            $q = $dbPlatform->getTruncateTableSql($classMetaData->getTableName());
	            $connection->executeUpdate($q);
	            $connection->query('SET FOREIGN_KEY_CHECKS=1');
	            $connection->commit();
	            return [
	                'code' => 1,
	                'msg' => '执行成功'
	            ];
	        }catch (\Exception $e) {
	            $connection->rollback();
	            return [
	                'code' => 0,
	                'msg' => '执行失败'
	            ];
	        }
	    }
	
	    public function data()
	    {
	        $Attachment = [
	            [
	                'gid' => '1',
	                'content' => 'aaaa'
	            ],
	            [
	                'gid' => '1',
	                'content' => 'bbbb'
	            ],
	        ];
	        return $Attachment;
	    }
	}
