## Command Excle表 导入命令
### 完整代码
	<?php
	
	namespace ApplyBundle\Command;
	
	use Symfony\Bundle\FrameworkBundle\Command\ContainerAwareCommand;
	use Symfony\Component\Console\Output\OutputInterface;
	use Symfony\Component\Console\Input\InputInterface;
	use OrganBundle\Entity\Lawfirm; //使用到的 数据表
	
	class LawfirmCommand extends ContainerAwareCommand
	{
	    protected function configure()
	    {
	        $this
	            ->setName('system:add:lawfirm') //命令
	            ->setDescription('add judicialcate information.')// 提示
	            ->setHelp("This command for add judicialcate information....");
	    }
	
	    protected function execute(InputInterface $input, OutputInterface $output)
	    {
	//        ini_set('memory_limit','-1');
	// 开始提示
	        $output->writeln([
	            'Start export data',
	            '---------------'
	        ]);
	        $file = './web/doc/lawfirm.xlsx'; // .xls 可能会报 内存溢出的错误
	        $objReader = \PHPExcel_IOFactory::createReader('excel2007');/*Excel5 for 2003 excel2007 for 2007*/
	        $objPHPExcel = $objReader->load($file); //Excel 路径
	        $objWorksheet = $objPHPExcel->getActiveSheet();
	        $highestRow = $objWorksheet->getHighestRow();   // 取得总行数
	        $highestColumn = $objWorksheet->getHighestColumn();
	        $highestColumnIndex = \PHPExcel_Cell::columnIndexFromString($highestColumn);//总列数
	        for ($row = 2; $row <= $highestRow; $row++) {
	            $data = array();
	//注意highestColumnIndex的列数索引从0开始
	            for ($col = 0; $col <= $highestColumnIndex - 1; $col++) {
	                $str = $objWorksheet->getCellByColumnAndRow($col, $row)->getValue();
	                if (is_object($str)) $str = $str->__toString();
	                $data[$col] = $str;
	
	            }
	
	            $res = $this->add($data, $output);
	            if ($res) {
	                $output->writeln([
	                    '' . $row . 'ROW SUCCESS'
	                ]);
	            }
	        }
	// 结束提示
	        $output->writeln([
	            '---------',
	            'End flush'
	        ]);
	    }
	
	    private function add($row, OutputInterface $output)
	    {
	        $doctrine = $this->getContainer()->get('doctrine');
	        $em = $doctrine->getManager();
	        set_time_limit(0);
	
	        if (isset($row[0]) && $row[0] != null) {
	
	            $em = $doctrine->getManager();
	            $Lawfirm = new Lawfirm();
	
	            $repo = $em->getRepository('OrganBundle:Authenast')->like($row[44]); //查询数据
	
	            if (!empty($repo[0])){
	                $Lawfirm->setZGJGID($repo[0]['id']);
	            }else{
	                $Lawfirm->setZGJGID($row[44]);
	            }
	
	            $em->persist($Lawfirm);
	            $em->flush();
	            return true;
	        }
	        return false;
	    }
	}