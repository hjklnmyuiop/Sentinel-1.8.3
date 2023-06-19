# Sentinel-1.8.3
Sentinel-1.8.3二次开发
修改文件：
1.sentinel-dashboard\src\main\java\com\alibaba\csp\sentinel\dashboard\controller\v2\FlowControllerV2中拉取，推送方式改用nacos

    @Autowired
    @Qualifier("flowRuleNacosProvider")
    private DynamicRuleProvider<List<FlowRuleEntity>> ruleProvider;
    @Autowired
    @Qualifier("flowRuleNacosPublisher")
    private DynamicRulePublisher<List<FlowRuleEntity>> rulePublisher;
2.将sentinel-dashbord项目中的sentinel-dashboard\src\test\java\com\alibaba\csp\sentinel\dashboard\rule\nacos文件夹拷贝到main文件夹下

3.修改NacosConfig文件，添加nacos服务信息
public class NacosConfig {

    @Bean
    public Converter<List<FlowRuleEntity>, String> flowRuleEntityEncoder() {
        return JSON::toJSONString;
    }

    @Bean
    public Converter<String, List<FlowRuleEntity>> flowRuleEntityDecoder() {
        return s -> JSON.parseArray(s, FlowRuleEntity.class);
    }

    @Bean
    public ConfigService nacosConfigService() throws Exception {
        Properties properties = new Properties();
        //nacos服务地址
        properties.setProperty("serverAddr","172.16.6.95:8848");
        //命名空间uuid
        properties.setProperty("namespace","899ae177-b497-4c34-9a1e-4ea438e23c89");

        return ConfigFactory.createConfigService(properties);
    }
}
