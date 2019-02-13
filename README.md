# p2-physics-Doc-Ch
p2物理引擎关键类属性的翻译


*world类的方法：
·addBody：添加刚体
·removeBody：移除刚体
·addConstraint：添加约束
·removeConstraint：移除约束
·addContactMaterial：添加接触材料
·removeContactMaterial：移除接触材料
·addSpring：添加弹簧
·removeSpring：移除弹簧
·clear：重置world，移除所有刚体、约束和弹簧
·enableBodyCollision：使两个刚体之间的碰撞行为有效
·disableBodyCollision：使两个刚体之间的碰撞行为无效
·getBodyById：获取刚体的id
·has：检查某个事件是否被监听
·hitTest：检查某个坐标点是否与某个刚体重合（碰撞）
·raycast：光线投射到世界上所有的物体上
·emit、on、off、has：分别表示派发、监听、取消监听，以及检测是否包含指定事件功能
·runNarrowphase：对一对shape进行精测计算
·setGlobalRelaxation：设置所有方程和接触材料的松弛模量
·setGlobalStiffness：设置所有方程和接触材料的硬度
·step：计算t秒后世界中所有刚体的位置。


*world类的属性：
·applyDamping<Boolean>：是否给世界每step添加阻尼
·applyGravity<Boolean>：是否给世界添加重力
·applySpringForces<Boolean>：是否给世界每step添加弹力
·bodies<Array>：指向被添加到world的所有刚体的数组
·BODY_SLEEPING<Number>：使刚体处于静默状态
·broadphase<Broadphase>：属于碰撞检测中的粗测阶段，获取检测到的每两个Shape相交的包围盒
·constraints<Array>：指向已经添加到world的所有约束
·contactMaterials<Array>：指向已经添加到world的所有接触材料
·defaultContactMaterial<ContactMaterial>：定义了两种材料接触时默认会发生什么，比如可以规定材料之间的摩擦系数。
·defaultMaterial <Material>：给世界指定默认材料
·emitImpactEvent<Boolean>：设置为true可以自动触发冲击力事件，关闭可提高性能
·frictionGravity<Number>：指定与重力相关的摩擦力
·gravity<Array>：使每step都有重力影响
·ISLAND_SLEEPING<Number>：岛屿中所有的刚体都将变成睡眠状态。islandSplit能切割岛屿实现活力
·islandManager<IslandManager>：岛屿管理器
·islandSpli<Boolean>t：是否允许岛屿分裂。岛状分裂对精度和性能都有好处。
·lastTimeStep<Number>：追踪最后一step的时间
·narrowphase<Narrowphase>：属于碰撞检测中的精测阶段。world.narrowphase.contactEquations 取得所有接触约束方程;world.narrowphase.frictionEquations 取得所有摩擦约束方程
·NO_SLEEPING<Number>：从不使刚体失效
·overlapKeeper<OverlapKeeper>：保持重叠
·sleepMode<Number>：睡眠模式
·solveConstraints<Boolean>：使每step的约束求解有效/无效
·solver<Solver>：求解器，用于求解约束和接触方程式。默认为高斯求解器
·springs<Array>：获取世界的所有弹簧
·stepping<Boolean>：判断当前是否在运行step()中 
·time<Number>：当前的世界时间
·useFrictionGravityOnZeroGravity<Boolean>：默认为true，如果重力为零，而useworldyasfrictiongravity =true，那么就改用frictiongravity来代替摩擦力。这种方式对于没有重力的游戏很有用
·useWorldGravityAsFrictionGravity<Boolean>：默认为true，frictionGravity将被自动设置为gravity


*world类的事件：
·addBody：当有body添加到world时触发
·addSpring：当有spring添加到world时触发
·beginContact：当world中有两个shape发生重叠或两个刚体发生碰撞时触发
·endContact：当world中有两个shape或body分离时触发
·impact：当两个刚体第一次碰撞时触发；该事件是在step完成后才触发的
·postBroadphase：碰撞粗检查完成收集时触发；你能任意修改Broadphase数组中的元素，取消一些你不想发生的碰撞
·postStep：step()完成后触发。常用于碰撞求解后触发，碰撞求解后的回调函数，需要计算碰撞冲击力造成的破坏等效果时，需要使用此事件。
·preSolve：在求解器解出方程式之间触发。可以控制哪些方程式需要放入求解器中进行求解，也可以在事件的回调函数中通过改变约束方程式中的属性来改变碰撞效果
·removeBody：body从world中移除时触发


*body类的方法：
·addShape：添加shape到body
·adjustCenterOfMass：调整重心位置。刚体中增加或移去形状后，重心并不会自动改变，可以使用adjustCenterOf Mass()方法，使刚体重心重新回到中心位置。这个方法不带任何参数，也没有返回值。 
·applyDamping：使用阻尼
·applyForce：作用力可以让刚体状态改变，通过applyForce()方法，可以在指定点worldPoint对刚体施加一个作用力，形成一个加速度或扭力，改变刚体的线速度或角速度。构造函数为：function applyForce(force:number[], worldPoint:number[]) 其中，force是要施加的作用力，这是一个二维向量； worldPoint是一个全局坐标点，表示force在刚体上的作用点，当此点不在刚体重心位置时，刚体角度也会发生变换。 
·applyForceLocal：
·applyImpulse：如果要瞬间改变刚体的状态，如子弹弹出膛效果，需要使用applyImpulse()对其施加冲量，使刚体的速度和角速度瞬间发生变化。 function applyImpulse(impulse:number[], relativePoint:number[]) 其中，impulse为要施加的冲量，冲量有大小和方向，是一个二维向量； relativePoint是一个本地坐标点，表示冲量的作用点，当作用点不在刚体中心时刚体角度也会发生变化。 
·applyImpulseLocal：
·emit、on、off、has：分别实现派发、监听、取消监听，以及检测是否包含指定事件功能。
·fromPolygon：fromPolygon用于将多边形分解成一个个小的形状，然后组合成完整的多边形。 function fromPolygon(path:number[][], [options]):boolean 其中，path保存了多边形顶点数组，options为可选属性，定义多边形分解的相关设置。
·getAABB：获取刚体的最小包围盒AABB(Axis Align Bounding Box)，最小包围盒AABB是指包围形状的最小矩形框。可以通过设置刚体的isDrawAABB=true，以显示出刚体的AABB。AABB对象将矩形对象的左下角和右下角坐标分别保存在属性lowerBound和upperBound中。 
·getArea：获取刚体的当前所有形状的面积总和
·getVelocityAtPoint：获取刚体中某一点的速度
·integrate：在给定当前速度的情况下，及时向前移动刚体
·overlaps：检测当前刚体与指定刚体是否有重叠，并返回检测结果
·removeShape：移除刚体的shape
·setDensity：设置刚体的密度
·setZeroForce：设置刚体上的力大小为0
·sleep：使刚体睡眠
·sleepTick：调用每个timestep更新内部睡眠计时器并在需要时更改睡眠状态。
·toLocalFrame：将全局坐标点转换成刚体本地坐标系统中的坐标点。通过这两个方法，可以实现本地坐标和全局坐标之间的转换。 
·toWorldFrame：将刚体本地坐标系统中的坐标点转换成全局坐标点
·updateAABB：更新刚体的AABB,同时设置aabbNeedsUpdate = false
·updateBoundingRadius：更新刚体的边界半径(this.boundingRadius)。如果任何形状、尺寸或位置发生了变化，就应该这样做。
·updateMassProperties：更新刚体惯性等数据。当改变身体的结构或质量时应该被调用。
·vectorToLocalFrame：将全局矢量转换成刚体本地矢量
·vectorToWorldFrame：将刚体本地矢量转换为全局矢量
·wakeUp：唤醒身体。通常你不需要这个，因为身体会在碰撞等事件中被自动唤醒。将睡眠状态设置为Body。唤醒并发出唤醒事件，如果身体之前没有醒来


*body类的属性：
·angle：角度
·area：面积
·axes：轴线
·body：一个shape只能被添加到唯一的body上
·boundingRadius：多边形的边界半径
·centerOfMass：质心
·collisionGroup：碰撞分组，与collisionMask一起使用，限制当前形状只与指定条件的形状发生碰撞
·collisionMask：为碰撞筛选，与collisionGroup一起使用，限制当前形状只与指定条件的形状发生碰撞
·collisionResponse：与其他物体接触时是否产生接触力,碰撞发生时当前形状会穿过碰撞刚体
·height：高
·id：id
·material：指定材质
·position：位置
·sensor：如果你想让这个形状成为一个传感器，设置为true。传感器不产生接触，但它仍然报告接触事件。如果你想知道一个形状是否与另一个形状重叠，而不需要它们产生接触，这是很有用。
·triangles：多边形的三角形。结构是由3个数组组成的数组，每个子数组包含3个整数，引用顶点。
·type：那种shape
·vertices：定点
·width：宽


*Shape：
·Shape是所有Shape拓展的基类
·常见的Shape拓展包括：圆形Circle、平面Plane、矩形Box、多边形Convex、粒子Particle、线段Line、胶囊Capsule、海拔形状HightField、射线Ray等
·Shape基类的属性包括：
  -- position：形状相对于本地坐标中心的偏移量，这个偏移量会影响刚体的重心
  -- angle：形状在刚体本地坐标系统中倾斜的角度
  -- collisionGroup：碰撞分组，与collisionMask一起使用，限制当前形状只与指定条件的形状发生碰撞
  -- collisionMask：为碰撞筛选，与collisionGroup一起使用，限制当前形状只与指定条件的形状发生碰撞
  -- sensor：设置形状是否为敏感区域，默认false，如果设置为true，则该形状不参与碰撞模拟，只作为感应区域
  -- collisionResponse：设置当与其他刚体发生碰撞时，当前形状是否进行碰撞模拟，默认true，如果设为false，碰撞发生时当前形状会穿过碰撞刚体
  -- type：为刚体类型，取值范围是Shape. CIRCLE和Shape.BOX等常量之一，不需要设置该参数，系统会自动设置。 
  -- material：为材质，是一个Material对象，用来设置形状发生碰撞时表现出的响应特性，如摩擦力、弹性系数等 
实际上，Material类中并不包含摩擦力、弹性系数等属性，它只是一个标志类，只是一个id，真正实现碰撞响应特性的是ContactMaterial类。ContactMaterial类用来为添加了merialA和merialB标识的两个形状设置独特的碰撞响应



*碰撞：
·复杂性存在于碰撞的处理中，而处理碰撞首先要检测到碰撞。碰撞检测最基本的方法就是两两刚体测试看其是否碰撞，这是不能满足效率要求的，因为每个刚体可能形状很复杂。为了进行快速碰撞检测，一般使用包围盒（Bounding Box，如AABB：Axis-Aligned Bounding Box、OBB：Oriented Bounding Box）技术，包围盒是一种简单几何体（长方体或球），刚体完全被其包含在里边。
·游戏中的碰撞分为两个阶段：broadphase和narrowphase
·broadphase：粗测阶段：两两刚体，测试其包围盒是否重叠（即包围盒的碰撞检测，因为包围盒是一种简单几何体，存在快速算法处理包围盒的碰撞检测）
·narrowphase：精测阶段：对于broadphase检测出的刚体对，再次进行刚体碰撞检测。任务为二：一是检测刚体之间是否碰撞；二手如果碰撞，计算出接触点（contact point）。
·碰撞——碰撞检测的4个阶段：
  -- postBroadphase：AABB开始发生重叠，但形状并没有发生接触 
  -- beginContact：刚体形状开始发生重叠，刚体继续保持原有速度移动 
  -- preSolve：刚体形状发生了重叠，但P2还未进行碰撞处理 
  -- endContact：P2已经完成了碰撞处理，并为碰撞刚体重新分配了速度，同时刚体形状分离，不再有重叠 
·碰撞——overlaps：检测当前刚体与指定刚体是否有重叠，并返回检测结果。只有当刚体不进行碰撞模拟时才需要使用overlaps，因为P2在实施碰撞检测和模拟时已经完成了这些内容。比如在界面中放置一个物体，当此位置已经存在物体时不能放置
·碰撞——collisionGroup和collisionMask：可以组合过滤掉不用参与碰撞的组合
·碰撞——disableBodyCollision：取消两个刚体之间的碰撞行为

*addContactMaterial（添加接触材料）：
·给不同材料之间添加摩擦系数、恢复、表面速度等属性
·例：  var iceMaterial = new p2.Material();
	
var steelMaterial = new p2.Material();
	
var iceCircleShape = new p2.Circle({ radius: 0.5 });

	var steelCircleShape = new p2.Circle({ radius: 0.5 });
	iceCircleShape.material = iceMaterial;

	var iceSteelContactMaterial = new p2.ContactMaterial(iceMaterial, steelMaterial, {
surfaceVelocity: -50,restitution: 0.5,friction: 0.3,stiffness: 1e7
});	
		
	world.addContactMaterial(iceSteelContactMaterial);

*Constraint（约束）：
·所有约束的基类
·常见的约束包括：
  -- 接触约束ContactConstraint：这是一个特别的约束，作用在于防止刚体之间的渗透重叠，并且它可以模拟摩擦和弹性。你无须创建这个约束，系统会自动创建它的。
  -- 距离关节DistanceConstraint：按照指定的距离distance将两个刚体约束在一起，其中任何一个刚体的位置发生变化，会牵着另一个刚体运动，以保证两者的间距为distance。但是两个刚体的角度不受约束，可以绕着节点旋转。
  -- 齿轮关节GearConstraint：按照指定的比例ratio，将两个刚体的角度angleA和angleB约束为angle=angleB*ratio。其中任何一个刚体的角度变换，都会牵着另一个刚体的角度变化，以确保两个刚体角度的比例为ratio。刚体的坐标位置不受约束，可以自由向任意方向移动。
  -- 锁定关节LockConstraint：将两个刚体绑定在一起，使其相对坐标位置、角度差保存不变，仿佛被钉在一起。此关节中的任何刚体坐标或角度发生变化，都会牵着另一个刚体的坐标和角度变化，以确保两个刚体相对坐标和角度分别为localOffsetB和localAngleB。
  -- 位移关节PrismaticConstraint：将刚体bodyB的运动方向，限定为在刚体bodyA本地坐标系统中的一个指定向量。
  -- 旋转关节RevoluteConstraint：限制两个刚体只能绕指定的控制点旋转，该控制点是刚体bodyA本地坐标系下的坐标。其中一个刚体的位置或角度发生变化时，为了确保控制点和刚体的相对位置不变，另一个刚体也会被牵制发生位置和角度的变化。旋转关节常用于模拟小车运动。

*Spring（弹簧）:
·属于约束的一种
·Spring只是抽象的父类，参与运动模拟的是两个子类LinearSpring（线性弹簧）和RotationalSpring（扭力弹簧）
·LinearSpring：是线性弹簧，对刚体的约束行为和距离关节DistanceConstraint相同，按照指定的距离restLength将两个刚体约束在一起，其中任何一个刚体的位置发生变化，会牵制着另一个刚体运动，以保证两者的间距为distance。在运动过程中，刚体bodyB呈现简谐运动。 
·RotationalSpring：是扭力弹簧，对刚体的约束类似齿轮关节，按照指定的restAngle约束两个刚体之间的角度差


*raycast（射线投射）：
·射线投射技术用于实现线段与形状的碰撞检测，通常用来模拟人物的视野、距离探测、子弹击中目标的检测，鼠标点击拾取物体等。
·function raycast(result:RaycastResult, ray:Ray)。其中，result是保存了碰撞刚体、碰撞点、碰撞距离等信息的一个RaycastResult对象；ray是用于检测的射线，是一个Ray类，包含起点from、终点to等属性。使用射线投射，需要用到Ray类和RaycastResult类。 


*Equation（约束方程式）：
·所有约束方程式的基类
·Equation用来保存碰撞发生时的碰撞点、碰撞向量等信息。一般用其两个子类ContactEquation和FrictionEquation来保存不同类型的信息
·ContactEquation：接触方程式（非穿透性约束方程式），即两个刚体必须接触。碰撞过程中，因接触而产生的碰撞信息，如碰撞点、碰撞向量、碰撞刚体等，都保存在ContactEquation对象中。
·FrictionEquation：摩擦方程式，碰撞过程中，因刚体相对运动而产生的摩擦等碰撞信息，保存在FrictionEquation对象中。



---------------------------------实战小结--------------------------------------
·坐标系：以egret为准，忽略p2坐标即可
·单位：以egret为准，忽略p2单位即可

·world.sleepMode = p2.World.BODY_SLEEPING	//设置刚体一定时间后自动进入睡眠状态以提高性能
·world.setGlobalRelaxation(number)	//设置默认粘稠度
·world.setGlobalStiffness(1e5)	//设置默认硬度，越硬大，碰撞越在表面
·world.defaultContactMaterial.friction = 0.5	//设置默认摩擦力设置为0.5
·world.defaultContactMaterial.restitution = 0
·world.narrowphase.contactEquations	//取得所有接触约束方程;
·world.narrowphase.frictionEquations	//取得所有摩擦约束方程；
·world.disableBodyCollision/enableBodyCollision(bodyA,bodyB)：取消和添加两个刚体之间的碰撞行为
·world.narrowphase.contactEquations	//是一个数组，其中存储了world中存在的所有接触方程

·ContactConstraint（接触约束）	//这是一个特别的约束，作用在于防止刚体之间的渗透重叠，并且它可以模拟摩擦和弹性。你无须创建这个约束，系统会自动创建它的。

·material和contactMaterial区别：material就是指材料；contactMaterial是设置不同材料之间接触时的参数
·切忌：material的id不能设置为0
·contactMaterial参数：{
	friction:number, //两个形状接触面的摩擦系数，默认0.3
	restitution:number, //两个形状碰撞时的弹性系数，默认0
	stiffness:number, //碰撞时形状表面的硬度，默认1000000，当stiffness较小时形状之间可以重叠并有一个排斥力促使分离，形成海绵或水平的弹性表面效果
	relaxation:number, //粘稠度，越小表现出刚体反弹越大。当形状之间重叠后，在一次碰撞模拟计算过程中实施排斥力的次数，可以想象成粘稠度，取值大时刚体速度衰减快
	frictionStuffness:number, 
	frictionRelaxation:number, 
	sufaceVelocity:number	//两个刚体接触时，接触面方向上两个刚体的相对速度，如果其中一个刚体静止，则sufaceVelocity表示另一个刚体的速度，常用来模拟传送带效果
}

·body.applyImpulse ( impulse [relativePoint] )	//给刚体添加一个推力，推力会改变刚体的速度和角速度；impulse Array:推力的向量;[relativePoint] Array optional:可选参数，指推力的着力点相对刚体质心的偏移量，默认为[0,0],即刚体的质心。
·body.velocity=[Array]	//刚体的速度，velocity[0]为x轴上的速度，velocity[1]为y轴上的速度
·body.type=
  -- p2.Body.STATIC——刚体不可以移动，但是可以与 dynamic类型刚体交互。
  -- p2.Body.DYNAMIC——刚体可以与任何类型的刚体交互，可以移动。
  -- p2.Body.KINEMATIC——刚体通过设置速度来控制，其他方面则和Static刚体相同。
  
  
  ----------------------------定义一个小球类实例----------------------------------
class Ball extends p2.Body {
    public constructor() {
        super({
            //速度相关属性
            position: [Main.StageWidth / 2, Main.StageHeight - 300],   //默认：[0, 0]。
            velocity: [0, 0],       //默认：[0, 0]。线性速度
            damping: 0,  //默认：0。线性速度阻尼,也称速度衰减系数，取值在0~1之间;damping可以用来模拟刚体与地面之间的摩擦力。
            fixedX: false,//默认：false。设定是否固定刚体的x坐标;设定了fixedX及fixedY属性后，需要调用updateMassProperties()方法，才能使其发挥作用
            fixedY: false,//默认：false。设定是否固定刚体的y坐标;设定了fixedX及fixedY属性后，需要调用updateMassProperties()方法，才能使其发挥作用
            force: [0, 0],//默认：[0,0]。表示刚体当前受到的作用力大小
            gravityScale: 1,//默认：1。设置当前刚体的重力加速度缩放比例
            //角度相关属性
            angle: 0,//默认：0。表示刚体角度，为弧度值，正值增大为顺时针旋转。 
            angularVelocity: 1,//默认：0。表示刚体角速度，即旋转速度
            angularDamping: 0.2,//默认：0.1。表示刚体角速度阻尼，取值范围0~1
            angularForce: 0,//默认：0表示刚体在角速度方向上受到扭力大小，单位N。
            fixedRotation: false,//默认：false。设置是否固定刚体角度
            //其他属性
            id: ID.BALL,
            type: p2.Body.DYNAMIC,  //默认：Body.STATIC。
            mass: 1,    //默认：0。
            ccdIterations: 10,   //默认：10。ccdIterations越大，碰撞模拟越精确，但计算效率也降低。
            ccdSpeedThreshold: -1,   //默认：-1。CCD碰撞检测的最低速度
            collisionResponse: true, //默认：true。是否会进行碰撞模拟,不进行碰撞模拟，会穿过刚体
            allowSleep: true,    //默认：-1。指定是否允许刚体进入睡眠状态
            sleepSpeedLimit: 0.2,//默认：0.2。为刚体进入睡眠状态时的最小速度
            sleepState: p2.Body.AWAKE,//默认：AWAKE。
            sleepimeLimit: 1,//默认：AWAKE。为刚体从瞌睡Body.SLEEPY状态进入睡眠状态Body.SLEEPING状态需要的时间
        })
        this.shapeInit()
        this.displayInit()
    }
    private shapeInit() {
        const shape: p2.Shape = new p2.Circle({
            radius: 40,
            // shape通用属性
            position: [0, 0],     //相对于刚体本地坐标中心的偏移量
            angle: 0,   //形状在刚体本地坐标系统中倾斜的角度
            collisionGroup: FiltCollisGroup.BALL, //碰撞分组，与collisionMask一起使用，限制当前形状只与指定条件的形状发生碰撞
            collisionMask: FiltCollisGroup.WALL | FiltCollisGroup.GROUND | FiltCollisGroup.DANGER | FiltCollisGroup.Star | FiltCollisGroup.Destination,  //为碰撞筛选，与collisionGroup一起使用，限制当前形状只与指定条件的形状发生碰撞,表示可与该刚体发生碰撞的碰撞组
            sensor: false,  //设置形状是否为敏感区域，默认false，如果设置为true，则该形状不参与碰撞模拟，只作为感应区域
            collisionResponse: true,    //设置当与其他刚体发生碰撞时，当前形状是否进行碰撞模拟，默认true，如果设为false，碰撞发生时当前形状会穿过碰撞刚体
            type: p2.Shape.CIRCLE,   //刚体类型，取值范围是Shape. CIRCLE和Shape.BOX等常量之一，不需要设置该参数，系统会自动设置
            material: P2MaterialMgr.getInstance().ballMaterial   //材质标识
        })
        this.addShape(shape)
    }
    private displayInit() {
        const display: egret.Bitmap = BusUtils.createIcon()
        this.displays = [display]
    }
}


