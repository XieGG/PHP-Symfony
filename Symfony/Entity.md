## 关于Entity的东西
###参数解释

    /**
     * @var string
     *
     * @ORM\Column(name="name", type="string", length=32, nullable=true, options={"comment": "名字"})
     * @Assert\NotBlank(message="名字不能为空")
     */
		
	nullable=true //数据表可以为空
	options={"comment": "名字"} //注释
 	@Assert\NotBlank(message="名字不能为空") //提示信息

###自动获取时间模板
	
    /**
     * @ORM\PrePersist()
     */
    public function prePersist()
    {
        if ($this->getCreateat() == null) {
            $this->setCreateat(new \DateTime());
        }
        $this->setUpdateat(new \DateTime());
    }

    /**
     * @ORM\PreUpdate()
     */
    public function preUpdate()
    {
        $this->setUpdateat(new \DateTime());
    }
	 

