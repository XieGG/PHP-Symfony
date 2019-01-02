## 关于Form
### 一个完整的Type
	<?php
	namespace MessageBundle\Form;
	
	use MessageBundle\Entity\Message;
	use Symfony\Bridge\Doctrine\Form\Type\EntityType;
	use Doctrine\ORM\EntityRepository;
	use Symfony\Component\Form\AbstractType;
	use Symfony\Component\Form\Extension\Core\Type\ChoiceType;
	use Symfony\Component\Form\FormBuilderInterface;
	use Symfony\Component\OptionsResolver\OptionsResolver;
	use Symfony\Component\Form\Extension\Core\Type\SubmitType;
	
	class MessageType extends AbstractType
	{
	    /**
	     *
	     * {@inheritdoc}
	     *
	     */
	    public function buildForm(FormBuilderInterface $builder, array $options)
	    {
	        $builder->add('title', null, [
	            'label' => '标题',
	            'attr' => [
	                'lay-verify' => "required",
	                'placeholder' => "请输入标题",
	                'autocomplete' => "off"
	            ]
	        ])
	            ->add('content', null, [
	            'label' => '内容',
	            'attr' => [
	                'lay-verify' => "required",
	                'placeholder' => "请输入内容",
	                'autocomplete' => "off"
	            ]
	        ])
	            ->add('status', ChoiceType::class, [ //选择 样式
	            'label' => '状态',
	            'choices' => [
	                '启用' => 1,
	                '禁用' => 0
	            ],
	            'expanded' => true, //开启 按钮样式 或者 复选框样式
	            'data' => $options['obj']->getStatus()
	        ])
	            ->add('projectCode', EntityType::class, [ //对Entity自定义查询
	            'label' => '项目',
	            'class' => 'RtuBundle:ApiApp',
	            'query_builder' => function (EntityRepository $er) {
	                return $er->createQueryBuilder('u')
	                    ->orderBy('u.appId', 'DESC');
	            },
	            'choice_label' => 'appName',
	            'attr' => [ //更改样式
	                'class' => 'input'
	            ]
	        ])
	            ->add('submit', SubmitType::class, [
	            'label' => '提交'
	        ]);
	    }
	
	    /**
	     *
	     * {@inheritdoc}
	     *
	     */
	    public function configureOptions(OptionsResolver $resolver)
	    {
	        $resolver->setDefaults(array(
	            'data_class' => 'MessageBundle\Entity\Message',
	            'obj' => new Message()
	        ));
	    }
	
	    /**
	     *
	     * {@inheritdoc}
	     *
	     */
	    public function getBlockPrefix()
	    {
	        return 'messagebundle_message';
	    }
	}
