## 完整的查询功能
### html 页面代码
	<form action="" method="get">
	    <select class="" name="type" onchange="this.form.submit();">
	        <option value="">所有设备</option>
	        <option value="1" {% if type=="1" %}selected="selected"{% endif %}>水位计</option> //刷新页面之后值不变
	        <option value="2" {% if type=="2" %}selected="selected"{% endif %}>电子水尺</option>
	        <option value="3" {% if type=="3" %}selected="selected"{% endif %}>流量计</option>
	        <option value="5" {% if type=="5" %}selected="selected"{% endif %}>雨量机</option>
	    </select>
	</form>
	<form action="" method="get">
	    <select class="" name="projectCode" onchange="this.form.submit();">
	        {% if name is defined and name is not empty %}
	            <option value="">所有项目</option>
	            {% for item in name %}
	                <option value="{{ item.appId }}" {% if project==item.appId %}selected{% endif %}>{{ project_name(item.appId) }}</option>
	            {% endfor %}
	        {% endif %}
	    </select>
	</form>
### Controller 代码
	public function indexAction(Request $request)
	    {
	        /**
	         *
	         * @var \RtuBundle\Repository\DeviceRepository $repo
	         */
	        $repo = $this->getDoctrine()->getRepository('RtuBundle:Device');
	        $pagesize = 15;
	        $paginator = $this->get('knp_paginator');
	        $project = $request->query->get('projectCode'); //获取到 get方法 传过来的参数
	        $type = $request->query->get('type'); 			//获取到 get方法 传过来的参数
	        $pagination = $paginator->paginate($repo->getPageQuery($project,$type), $request->query->getInt('page', 1), $pagesize);
	        $name = $this->getDoctrine()->getRepository('RtuBundle:ApiApp')->findAll();
	
	        return $this->render('@Rtu/Device/index.html.twig', array(
	            'pagination' => $pagination,
	            'project' => $project,
	            'type' => $type,
	            'name' => $name
	        ));
	    }
### Repository 代码
    public function getPageQuery($project,$type)
    {
        if (!empty($project)){
            return $this->createQueryBuilder('a')
                ->where('a.projectCode= :project')
                ->setParameter('project', $project)
                ->orderBy('a.id','DESC')
                ->getQuery();
        }else if(!empty($type)){
            return $this->createQueryBuilder('a')
                ->where('a.type= :type')
                ->setParameter('type', $type)
                ->orderBy('a.id','DESC')
                ->getQuery();
        }else{
            return $this->createQueryBuilder('a')
                ->orderBy('a.id','DESC')
                ->getQuery();
        }
    }