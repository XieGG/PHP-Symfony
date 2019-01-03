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
	            ->add('status', ChoiceType::class, [		//选择下拉菜单，单选按钮和复选框
	            'label' => '状态',
	            'choices' => [
	                '启用' => 1,
	                '禁用' => 0
	            ],
	            'expanded' => true,	//开启 单选按钮
	            'data' => $options['obj']->getStatus()
	        ])
	            ->add('projectCode', EntityType::class, [ //对 Entity 使用自定义查询
	            'label' => '项目',
	            'class' => 'RtuBundle:ApiApp',
	            'query_builder' => function (EntityRepository $er) {
	                return $er->createQueryBuilder('u')
	                    ->orderBy('u.appId', 'DESC');
	            },
	            'choice_value' => function ($entity = null) {
	                return $entity ? $entity->getAppId() : '';
	            },
	            'choice_label' => 'appName',	//显示出来的文本
	            'attr' => [
	                'class' => 'input'	//更改下拉框样式
	            ]
	        ])
	            ->add('submit', SubmitType::class, [ //按钮
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
	            'obj' => ''
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
### Controller相关代码

    public function editAction(Request $request)
    {
        $request->attributes->set('_route',"message_message_index");
        $errors =[];
        $id = $request->query->getInt('id');
        /**
         * @var \MessageBundle\Entity\Message $message
         */
        $message = $this->getDoctrine()
            ->getRepository('MessageBundle:Message')
            ->find($id);
        if (empty($message)){
            return $this->msgResponse(1,'警告','该记录不存在或已被删除','message_message_index');
        }
		#<---赋值，更改默认值--->
        $project = $this->getDoctrine()->getRepository('RtuBundle:ApiApp')->findOneBy([
            'appId'=>$message->getProjectCode(),
        ]);
        $message->setProjectCode($project);
        $form = $this->createForm('MessageBundle\Form\MessageType',$message,[
           'obj' => $message
        ]);
		#<------>
        $form->handleRequest($request);
        if ($form->isSubmitted()){
            if ($form->isValid()){
                $message->setProjectCode($message->getProjectCode() ? $message->getProjectCode()->getAppId() : '1234567890');
                $this->getDoctrine()
                    ->getManager()
                    ->flush();
                return $this->msgResponse(0,'提示','修改成功','message_message_index');
            }else{
                $errors = $this->serializeFormErrors($form);
            }
        }
        return $this->render('@Message/Message/edit.html.twig', array(
            'message' => $message,
            'form' => $form->createView(),
            'errors' => $errors
        ));
    }
