# Run a Spring Batch Job With Quartz  

# 通过Quatrz来运行Spring批量任务  

## Want to learn how to run a Spring Batch job with Quartz? Check out this tutorial to learn how to with the Quartz scheduler and sample code!    

## 想知道如何用Quartz来运行一个Spring的批量任务？看看这篇教程来学习如何用Quartz调度程序来运行Spring批量任务，此外还有示例代码！    

---

Hey, folks. In this tutorial, we will see how a Spring Batch job runs using a Quartz scheduler.
If you are not sure about the basics of Spring Batch, you can visit my tutorial [here](https://dzone.com/articles/spring-batch).   

在这篇教程中，我们将会理解如何用Quartz调度程序来运行Spring批量任务。
如果你对Spring Batch的基础不是很好的话可以访问我的[这篇教程](https://dzone.com/articles/spring-batch)    

Now, as we know, Spring Batch jobs are used whenever we want to run any business-specific code or run/generate any reports 
at any particular time/day. There are two ways to implement jobs: `tasklet`  and `chunks` . 
In this tutorial, I will create a simple job using a `tasklet`, which will print a  `logger`.   

正如我们现在所了解的，无论何时我们想运行任何处理特定事情的代码，或者在任何特定的时间来处理或生成任何报告都会用到Spring Batch任务。
这有两种途径实现任务： `tasklet` 和 `chunks` 。在这篇教程中我将会使用 `tasklet` 来创建一个简单的任务，这个任务将用来输出 `logger`  

The basic idea here is what all configurations are required to make this job run. 
We will use Spring Boot to bootstrap our application.    

第一步就是配置可以使这个任务可以运行的所有的所需的配置，我们将使用Spring Boot来独立创建我们的应用。    

#### We require below two dependencies in pom.xml for having Spring Batch and Quartz in our application.     

#### 在我们的应用中需要在 pom.xml 文件中添加下面的两项依赖来添加 Spring Batch 和 Quartz


```xml
<!-- https://mvnrepository.com/artifact/org.springframework.batch/spring-batch-core -->
<dependency>
    <groupId>org.springframework.batch</groupId>
    <artifactId>spring-batch-core</artifactId>
    <version>4.0.1.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.quartz-scheduler/quartz -->
<dependency>
    <groupId>org.quartz-scheduler</groupId>
    <artifactId>quartz</artifactId>
    <version>2.3.0</version>
</dependency>
```

#### Now, let's see what all configurations we require in our code to run the job.    

#### 现在我们来看看在我们的代码中所需的所有配置来运行这个任务   

##### 1. BatchConfiguration.java:  

```java
package com.category.batch.configurations;
import org.springframework.batch.core.configuration.JobRegistry;
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.batch.core.configuration.support.ApplicationContextFactory;
import org.springframework.batch.core.configuration.support.AutomaticJobRegistrar;
import org.springframework.batch.core.configuration.support.DefaultJobLoader;
import org.springframework.batch.core.configuration.support.JobRegistryBeanPostProcessor;
import org.springframework.batch.core.configuration.support.MapJobRegistry;
import org.springframework.batch.core.launch.JobLauncher;
import org.springframework.batch.core.launch.support.SimpleJobLauncher;
import org.springframework.batch.core.repository.JobRepository;
import org.springframework.batch.core.repository.support.MapJobRepositoryFactoryBean;
import org.springframework.batch.support.transaction.ResourcelessTransactionManager;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
@Configuration
@EnableBatchProcessing
@Import({
    BatchJobsDetailedConfiguration.class
})
public class BatchConfiguration {
    @Bean
    public JobRegistry jobRegistry() {
        return new MapJobRegistry();
    }
    @Bean
    public ResourcelessTransactionManager transactionManager() {
        return new ResourcelessTransactionManager();
    }
    @Bean
    public JobRepository jobRepository(ResourcelessTransactionManager transactionManager) throws Exception {
        MapJobRepositoryFactoryBean mapJobRepositoryFactoryBean = new MapJobRepositoryFactoryBean(transactionManager);
        mapJobRepositoryFactoryBean.setTransactionManager(transactionManager);
        return mapJobRepositoryFactoryBean.getObject();
    }
    @Bean
    public JobLauncher jobLauncher(JobRepository jobRepository) throws Exception {
        SimpleJobLauncher simpleJobLauncher = new SimpleJobLauncher();
        simpleJobLauncher.setJobRepository(jobRepository);
        simpleJobLauncher.afterPropertiesSet();
        return simpleJobLauncher;
    }
    @Bean
    public JobRegistryBeanPostProcessor jobRegistryBeanPostProcessor(JobRegistry jobRegistry) {
        JobRegistryBeanPostProcessor jobRegistryBeanPostProcessor = new JobRegistryBeanPostProcessor();
        jobRegistryBeanPostProcessor.setJobRegistry(jobRegistry());
        return jobRegistryBeanPostProcessor;
    }
}
```

Let's go one-by-one:  

+ `@Configuration`: This specifies that this class will contain beans and will be instantiated at load time.  

+ `@EnableBatchProcessing`: This enables the Spring Batch features and provides a base configuration for setting up batch jobs.  

+ `@Import({BatchJobsDetailedConfiguration.class})`: This will import some other configurations required,
which we will see later.  

+ `JobRegistry`: This interface is used to register the jobs.  

+ `ResourcelessTransactionManager`: This class is used when you want to run the job using any database persistence.  

+ `JobRepository`: This contains all the metadata of the job, which returns a  `MapJobRepositoryFactoryBean` used for non-persistent DAO implementations.  

+ `JobLauncher`: This is used to launch a job, requires jobRepository as a dependency.  

+ `JobRegistryBeanPostProcessor`: This is used to register a job in the `jobRegistry`, which returns the  `jobRegistry`.    

我们一步一步的看：  

+ `@Configuration`： 这说明这个被它标注的类将会包含 beans 而且会在加载时初始化。  

+ `@EnableBatchProcessing`：这使得 Spring Batch 的特性得以启用并且为设置批量任务提供了一个基础的配置。  

+ `@Import({BatchJobsDetailedConfiguration.class})`： 这将会导入一些其他的所需配置，我们将会在稍后看到他。  

+ `JobRegistry`： 用来注册任务的接口。  

+ `ResourcelessTransactionManager`：这个类在你想用任何数据库持久性来运行任务时使用  

+ `JobRepository`：这包含了任务的元数据，这将会返回一个 `MapJobRepositoryFactoryBean` ，这个bean用来实现非持久性的DAO  

+ `JobLauncher`：这个被用来加载一个任务，需要依赖 jobRepository    

+ `JobRegistryBeanPostProcessor`: 这个用来在 `jobRegistry` 里注册任务的，并返回 `jobRegistry`。  

Let's go to the imported class now.  

我们现在来看看导入的类  

##### 2. BatchJobsDetailedConfiguration.java:

```java
package com.category.batch.configurations;
import java.util.HashMap;
import java.util.Map;
import org.springframework.batch.core.configuration.JobRegistry;
import org.springframework.batch.core.configuration.support.ApplicationContextFactory;
import org.springframework.batch.core.configuration.support.GenericApplicationContextFactory;
import org.springframework.batch.core.launch.JobLauncher;
import org.springframework.batch.core.launch.NoSuchJobException;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.quartz.CronTriggerFactoryBean;
import org.springframework.scheduling.quartz.JobDetailFactoryBean;
import org.springframework.scheduling.quartz.SchedulerFactoryBean;
import com.category.batch.job.JobLauncherDetails;
import com.category.batch.reports.config.ReportsConfig;
@Configuration
public class BatchJobsDetailedConfiguration {
    @Autowired
    private JobLauncher jobLauncher;
    @Bean(name = "reportsDetailContext")
    public ApplicationContextFactory getApplicationContext() {
        return new GenericApplicationContextFactory(ReportsConfig.class);
    }
    @Bean(name = "reportsDetailJob")
    public JobDetailFactoryBean jobDetailFactoryBean() {
        JobDetailFactoryBean jobDetailFactoryBean = new JobDetailFactoryBean();
        jobDetailFactoryBean.setJobClass(JobLauncherDetails.class);
        jobDetailFactoryBean.setDurability(true);
        Map < String, Object > map = new HashMap < > ();
        map.put("jobLauncher", jobLauncher);
        map.put("jobName", ReportsConfig.jobName);
        jobDetailFactoryBean.setJobDataAsMap(map);
        return jobDetailFactoryBean;
    }
    @Bean(name = "reportsCronJob")
    public CronTriggerFactoryBean cronTriggerFactoryBean() {
        CronTriggerFactoryBean cronTriggerFactoryBean = new CronTriggerFactoryBean();
        cronTriggerFactoryBean.setJobDetail(jobDetailFactoryBean().getObject());
        cronTriggerFactoryBean.setCronExpression("0 0/1 * 1/1 * ? *");
        return cronTriggerFactoryBean;
    }
    @Bean
    public SchedulerFactoryBean schedulerFactoryBean(JobRegistry jobRegistry) throws NoSuchJobException {
        SchedulerFactoryBean schedulerFactoryBean = new SchedulerFactoryBean();
        schedulerFactoryBean.setTriggers(cronTriggerFactoryBean().getObject());
        schedulerFactoryBean.setAutoStartup(true);
        Map < String, Object > map = new HashMap < > ();
        map.put("jobLauncher", jobLauncher);
        map.put("jobLocator", jobRegistry);
        schedulerFactoryBean.setSchedulerContextAsMap(map);
        return schedulerFactoryBean;
    }
}
```

Let's dig into this:

+ `ApplicationContextFactory`: This interface is primarily useful when creating a new `ApplicationContext` per execution of 
a job. It's better to create a sepearte `applicationContext` for each job.

+ `JobDetailFactoryBean`: This is used to create a Quartz job detail instance. This class will set a job class, 
which we will see later. It creates a map that will define and set the job name using a class and `joblauncher`.

+ `CronTriggerFactoryBean`: This is used to create a `Quartz` cron trigger instance. 
This will set the `jobDetail` created earlier and then the `cron` expression when this job will run
You can set the `cron` expressions as per your need. Cron expressions can be calculated from http://cronmaker.com.

+ `SchedulerFactoryBean`: This is used to create a Quartz scheduler instance and allows for the registration of `JobDetails` ,
`Calendars` , and `Triggers`, automatically starting the scheduler on initialization and shutting it down on destruction.

让我们详细了解一下这些：  

+ `ApplicationContextFactory`：当每个执行任务创建一个新的 `ApplicationContext` 时通常都用这个接口。  

+ `JobDetailFactoryBean`：这个被用来创建一个 `Quartz` 任务的具体实例，这个类将用于设置一个任务类。  

+ `CronTriggerFactoryBean`：这个被用来创建一个 `Quartz` 定时触发器实例，它将使 `jobDetail` 提前创建好，然后你可以自己
设置`cron` 表达式来使你的任务得以运行。  定时任务表达式可以在这里 http://cronmaker.com 计算

+ `SchedulerFactoryBean`：这个被用来创建一个 Quartz 调度程序实例，考虑到 `JobDetails` ,`Calendars` , 和  `Triggers` 的注册，以及在初始化时
自动开启调度程序和销毁时自动关闭，也需要用到它  

Let's check out the `JobLauncherDetails` class:  

我们来看看 `JobLauncherDetails` 这个类：  

##### 3. JobLauncherDetails.java:  

```java
package com.category.batch.job;
import java.util.Map;
import org.quartz.JobDataMap;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.JobParameters;
import org.springframework.batch.core.JobParametersBuilder;
import org.springframework.batch.core.JobParametersInvalidException;
import org.springframework.batch.core.configuration.JobLocator;
import org.springframework.batch.core.configuration.JobRegistry;
import org.springframework.batch.core.launch.JobLauncher;
import org.springframework.batch.core.launch.NoSuchJobException;
import org.springframework.batch.core.repository.JobExecutionAlreadyRunningException;
import org.springframework.batch.core.repository.JobInstanceAlreadyCompleteException;
import org.springframework.batch.core.repository.JobRestartException;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.quartz.QuartzJobBean;
public class JobLauncherDetails extends QuartzJobBean {
    static final String JOB_NAME = "jobName";
    public void setJobLocator(JobLocator jobLocator) {
        this.jobLocator = jobLocator;
    }
    public void setJobLauncher(JobLauncher jobLauncher) {
        this.jobLauncher = jobLauncher;
    }
    private JobLocator jobLocator;
    private JobLauncher jobLauncher;
    @Override
    protected void executeInternal(JobExecutionContext jobExecutionContext) throws JobExecutionException {
        JobParameters jobParameters = new JobParametersBuilder().addLong("time", System.currentTimeMillis()).toJobParameters();
        try {
            Map < String, Object > jobDataMap = jobExecutionContext.getMergedJobDataMap();
            String jobName = (String) jobDataMap.get(JOB_NAME);
            jobLauncher.run(jobLocator.getJob(jobName), jobParameters);
        } catch (JobExecutionAlreadyRunningException | JobRestartException | JobInstanceAlreadyCompleteException
            |
            JobParametersInvalidException e) {
            e.printStackTrace();
        } catch (NoSuchJobException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

This class has an overridden `executeInternal` method of the `QuartzJobBean` class, 
which takes the `jobdetails` from the map, which were already set before some of the `jobParameters` ,
and then executes the `jobLauncher.run()` to run the job as seen in the code.  

这个类重写了 `QuartzJobBean` 类的 `executeInternal` 方法。从一个 map 里获取早已在 `jobParameters` 里设置好了的  `jobdetials` ，
然后正如代码中看到的，执行 `jobLauncher.run()` 来运行任务   

Lets visit the ReportsConfig class.  

我们来看看 ReportsConfig class。

##### 4.ReportsConfig.java:  

```java
package com.category.batch.reports.config;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.DuplicateJobException;
import org.springframework.batch.core.configuration.JobRegistry;
import org.springframework.batch.core.configuration.annotation.JobBuilderFactory;
import org.springframework.batch.core.configuration.annotation.StepBuilderFactory;
import org.springframework.batch.core.configuration.support.ReferenceJobFactory;
import org.springframework.batch.core.step.tasklet.Tasklet;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import com.category.batch.reports.tasklet.ReportTasklet;
@Configuration
public class ReportsConfig {
    @Autowired
    private JobRegistry jobRegistry;
    public final static String jobName = "ReportsJob1";
    public JobBuilderFactory getJobBuilderFactory() {
        return jobBuilderFactory;
    }
    public void setJobBuilderFactory(JobBuilderFactory jobBuilderFactory) {
        this.jobBuilderFactory = jobBuilderFactory;
    }
    @Autowired
    private Tasklet taskletstep;
    @Autowired
    private JobBuilderFactory jobBuilderFactory;
    @Autowired
    private StepBuilderFactory stepBuilderFactory;
    @Bean
    public ReportTasklet reportTasklet() {
        return new ReportTasklet();
    }
    @Bean
    public Job job() throws DuplicateJobException {
        Job job = getJobBuilderFactory().get(jobName).start(getStep()).build();
        return job;
    }
    @Bean
    public Step getStep() {
        return stepBuilderFactory.get("step").tasklet(reportTasklet()).build();
    }
}
```

The main purpose of the class is having configurations related to each job.
You will have a separate config for each job as this. As you can see, 
we create the `tasklet` here, which we will see later. Also, we define and return the Job,
step using a `JobBuilderFactory`,  and `StepBuilderFactory`. 
These factories will automatically set the `JobRepository` for you.  

这个类的主要目的是把配置关联到每个任务。在这个类里每个任务都有各自的配置。正如你看到的，此处我们创建了 `tasklet`，我们稍后会看到它。
我们也定义并返回了一个任务，接着用 `JobBuilderFactory`, 和  `StepBuilderFactory` ，这些工厂类会自动为你设置 `jobRepository`。  

Let's go to the ReportTasklet, which is our job to be run.  

来看看我们将要运行的任务 `ReportTasklet`。  

##### 5. ReportTasklet.java:  

```java
package com.category.batch.reports.tasklet;
import java.util.logging.Logger;
import org.springframework.batch.core.StepContribution;
import org.springframework.batch.core.scope.context.ChunkContext;
import org.springframework.batch.core.step.tasklet.Tasklet;
import org.springframework.batch.repeat.RepeatStatus;
import org.springframework.context.annotation.Configuration;
@Configuration
public class ReportTasklet implements Tasklet {
    private static final Logger logger = Logger.getLogger(ReportTasklet.class.getName());
    @Override
    public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) {
        try {
            logger.info("Report's Job is running. Add your business logic here.........");
        } catch (Exception e) {
            e.printStackTrace();
        }
        return RepeatStatus.FINISHED;
    }
}
```

This class has a execute method that will be ran when the job is ran through the `jobLauncher.run()` from the `JobLauncherDetails` class. 
You can define your business logic that needs to be executed here.  

当任务执行到 `JobLauncherDetails` 的 `jobLauncher.run()` 方法，上面的这个类的方法将被执行。  

We will need some configuration in application.properties as below:  

我们需要在 application.properties 做如下配置：  

##### 6.  application.properties   

```
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
spring.batch.job.enabled=false
```

The first property is required to disable the datasource— only for testing purposes and is not required in production.  

第一个属性被用来禁用数据源——只有使用于测试环境不用于生产环境   

The second property is when before the server starts, the job is run. To avoaid this, we require this property.   

当服务开始时，任务会运行，为了避免此种情况，我们设置第二个属性  

Now, finally, let's go to the application class. This should be self-explanatory.  

最终，我们来看看应用类，这应该不用多说了。

##### 7. BatchApplication.java:  

```java
package com.category.batch;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
import org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration;
@SpringBootApplication(exclude={DataSourceAutoConfiguration.class})
public class BatchApplication {
    public static void main(String[] args) {
        SpringApplication.run(BatchApplication.class, args);
	}
}
```

Enough Configurations! Let's run this application and see the output. 
We have set the cron to 1 minute. After 1 minute, the job will be run.  

足够的配置可以运行任务了，让我们看看输出。我们设置了一分钟的定时任务。1分钟之后任务将会运行。  


```
2018-09-14 11:04:18.648 INFO 7008 --- [ost-startStop-1] org.quartz.impl.StdSchedulerFactory : Quartz scheduler 'schedulerFactoryBean' initialized from an externally provided properties instance.
2018-09-14 11:04:18.648 INFO 7008 --- [ost-startStop-1] org.quartz.impl.StdSchedulerFactory : Quartz scheduler version: 2.3.0
2018-09-14 11:04:18.653 INFO 7008 --- [ost-startStop-1] org.quartz.core.QuartzScheduler : JobFactory set to: org.springframework.scheduling.quartz.AdaptableJobFactory@b29e7d6
2018-09-14 11:04:19.578 INFO 7008 --- [ost-startStop-1] o.s.w.s.handler.SimpleUrlHandlerMapping : Mapped URL path [/**/favicon.ico] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-09-14 11:04:20.986 INFO 7008 --- [ost-startStop-1] s.w.s.m.m.a.RequestMappingHandlerAdapter : Looking for @ControllerAdvice: org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext@74a6e16d: startup date [Fri Sep 14 11:04:12 IST 2018]; root of context hierarchy
2018-09-14 11:04:21.264 INFO 7008 --- [ost-startStop-1] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error]}" onto public org.springframework.http.ResponseEntity<java.util.Map<java.lang.String, java.lang.Object>> org.springframework.boot.autoconfigure.web.servlet.error.BasicErrorController.error(javax.servlet.http.HttpServletRequest)
2018-09-14 11:04:21.268 INFO 7008 --- [ost-startStop-1] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],produces=[text/html]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.servlet.error.BasicErrorController.errorHtml(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
2018-09-14 11:04:21.356 INFO 7008 --- [ost-startStop-1] o.s.w.s.handler.SimpleUrlHandlerMapping : Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-09-14 11:04:21.356 INFO 7008 --- [ost-startStop-1] o.s.w.s.handler.SimpleUrlHandlerMapping : Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-09-14 11:04:22.526 INFO 7008 --- [ost-startStop-1] o.s.j.e.a.AnnotationMBeanExporter : Registering beans for JMX exposure on startup
2018-09-14 11:04:22.555 INFO 7008 --- [ost-startStop-1] o.s.c.support.DefaultLifecycleProcessor : Starting beans in phase 2147483647
2018-09-14 11:04:22.556 INFO 7008 --- [ost-startStop-1] o.s.s.quartz.SchedulerFactoryBean : Starting Quartz Scheduler now
2018-09-14 11:04:22.556 INFO 7008 --- [ost-startStop-1] org.quartz.core.QuartzScheduler : Scheduler schedulerFactoryBean_$_NON_CLUSTERED started.
2018-09-14 11:04:22.578 INFO 7008 --- [ost-startStop-1] com.category.batch.ServletInitializer : Started ServletInitializer in 17.386 seconds (JVM running for 26.206)
2018-09-14 11:04:23.395 INFO 7008 --- [ main] org.apache.coyote.ajp.AjpNioProtocol : Starting ProtocolHandler ["ajp-nio-8009"]
2018-09-14 11:04:23.399 INFO 7008 --- [ main] org.apache.catalina.startup.Catalina : Server startup in 24866 ms
2018-09-14 11:05:02.889 INFO 7008 --- [ryBean_Worker-1] o.s.b.c.l.support.SimpleJobLauncher : Job: [SimpleJob: [name=ReportsJob1]] launched with the following parameters: [{time=1536903301155}]
2018-09-14 11:05:03.262 INFO 7008 --- [ryBean_Worker-1] o.s.batch.core.job.SimpleStepHandler : Executing step: [step]
2018-09-14 11:05:03.503 INFO 7008 --- [ryBean_Worker-1] c.c.batch.reports.tasklet.ReportTasklet : Report's Job is running. Add your business logic here.........
2018-09-14 11:05:03.524 INFO 7008 --- [ryBean_Worker-1] o.s.b.c.l.support.SimpleJobLauncher : Job: [SimpleJob: [name=ReportsJob1]] completed with the following parameters: [{time=1536903301155}] and the following status: [COMPLETED]
```

The bold lines indicate your job ran and completed successfully. That's all for this tutorial. 
Please comment if you would like to add anything. Happy learning!  

加粗线部分的输出表明你的任务已经成功地完成了运行。这就是此篇教程的全部内容了。如果你想加些其他的请在下方评论，欢迎学习！
