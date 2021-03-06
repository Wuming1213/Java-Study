# Java-Study

### 对excel的数据处理的使用工具包

1、easyexcel
https://github.com/alibaba/easyexcel
https://www.yuque.com/easyexcel/doc/easyexcel


2、POI
https://iowiki.com/apache_poi/apache_poi_index.html


3、hutool 参考文档
https://hutool.cn/docs


https://github.com/coder-fang/thatblog
https://github.com/asiL-tcefreP/blog/tree/ai

https://juejin.cn/post/6926349072945283085
https://segmentfault.com/a/1190000023939043
https://xie.infoq.cn/article/6119a149d4704f27b191689e8
https://zhuanlan.zhihu.com/p/432055767
https://juejin.cn/post/6856555431570735117

https://juejin.cn/post/7000687334123372552


@Test
    public void write() {
        String fileName = "D:\\Study\\excel\\my_word.xlsx";
        String text = "词汇突破：\n\n" +
                "A separate survey：另一项调查\n\n" +
                "faculty and staff：教职员工\n\n" +
                "push through：强行推广\n\n" +
                "cost-cutting measures：减费措施\n\n" +
                "respondent：受访者\n\n" +
                "publicly funded：公众出资的\n\n" +
                "academic institutions ：学术机构\n\n" +
                "businesses：加了复数S就是公司的意思\n\n" +
                "in the numbers：大量的\n\n\n\n" +
                "句子解析：\n\n" +
                "黄色是主干（如果有宾语从句就用蓝色单独标记）\n\n" +
                "红色是定语\n\n" +
                "绿色是状语\n\n" +
                "紫色是同位语\n\n" +
                "第一句：\n\n" +
                "A separate survey of more than 1,000 UK faculty and staff members carried out between June and August last year revealed a sense that university leaders are using the pandemic as an excuse to push through cost-cutting measures.\n\n" +
                "参考译文：去年6月至8月期间对1000多名英国教职员进行的另一项调查显示，大学的领导者正以疫情为借口，推进削减成本的措施。\n\n" +
                "第二句：\n\n" +
                "Many respondents were also concerned// that publicly funded academic institutions are increasingly being run as businesses.\n\n" +
                "参考译文：许多受访者还担心，公立的学术机构越来越被运营得像企业。\n\n" +
                "第三句：\n\n" +
                "But if universities truly were like businesses, the survey findings suggest, staff would probably be happier — and would not be looking to leave in the numbers they seem to be.\n\n" +
                "参考译文：但调查结果显示，如果大学真的和企业一样，员工们可能会更快乐——并且不会有这么多人像看起来的那样希望离开。";

        // 进行切割
        List<MyWord> myWords = getMyWords(text);
        
        ExcelWriter excelWriter = null;
        try {
            excelWriter = EasyExcel.write(fileName, MyWord.class).build();
            WriteSheet writeSheet = EasyExcel.writerSheet("生词本").build();
            excelWriter.write(myWords, writeSheet);
        } finally {
            // 千万别忘记finish 会帮忙关闭流
            if (excelWriter != null) {
                excelWriter.finish();
            }

        }
    }

    private List<MyWord> getMyWords(String text) {
        final String split_one =  "\n\n\n\n"; 
        final String split_two = "\n\n";
        final String split_three = "：";
        String[] split = text.split(split_one);
        String words = split[0];
        String[] wordList = words.split(split_two);
        List<MyWord> myWords = Arrays.stream(wordList).skip(1).map(s -> {
            String[] split1 = s.split(split_three);
            MyWord myWord = new MyWord();
            myWord.setNewWord(split1[0]);
            myWord.setParaphrase(split1[1]);
            return myWord;
        }).collect(Collectors.toList());
        
        String sentence = split[1];
        Map<String, String> sentenceMap = new HashMap<>();
        List<String> sentenceList = Arrays.stream(sentence.split(split_two)).skip(5).collect(Collectors.toList());

        for (int i = 0; i < sentenceList.size(); i = i+3) {
            sentenceMap.put(sentenceList.get(i+1), sentenceList.get(i+2));
        }
        Set<String> strings = sentenceMap.keySet();
        for (String string : strings) {
            myWords.forEach(myWord -> {
                if (string.contains(myWord.getNewWord())) {
                    myWord.setMyNotes(sentenceMap.get(string));
                }
            });
        }
        return myWords;
    }
    
    
 
 
 
 
<repositories>
        <repository>
            <id>alimaven</id>
            <name>aliyun maven</name>
            <url>https://maven.aliyun.com/repository/central</url>
        </repository>
        <repository>
            <id>central-repos</id>
            <name>Central Repository</name>
            <url>http://repo.maven.apache.org/maven2</url>
        </repository>
    </repositories>
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <elasticsearch.version>7.7.1</elasticsearch.version>
    </properties>
    <dependencies>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <!-- excel 表格的依赖-->
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
            <version>4.0.1</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-freemarker</artifactId>
        </dependency>

        <!-- 阿里的excel依赖 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>easyexcel</artifactId>
            <version>3.0.5</version>
        </dependency>

        <!--neo4j的依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-neo4j</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
<!--            <exclusions>-->
<!--                <exclusion>-->
<!--                    <groupId>org.junit.vintage</groupId>-->
<!--                    <artifactId>junit-vintage-engine</artifactId>-->
<!--                </exclusion>-->
<!--            </exclusions>-->
        </dependency>


        <!--elasticsearch 与springboot的依赖-->
<!--        <dependency>-->
<!--            <groupId>org.springframework.boot</groupId>-->
<!--            <artifactId>spring-boot-starter-data-elasticsearch</artifactId>-->
<!--        </dependency>-->

        <dependency>
            <groupId>org.elasticsearch.client</groupId>
            <artifactId>elasticsearch-rest-high-level-client</artifactId>
            <version>${elasticsearch.version}</version>
            <exclusions>
                <exclusion>
                    <groupId>org.elasticsearch</groupId>
                    <artifactId>elasticsearch</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.elasticsearch.client</groupId>
                    <artifactId>elasticsearch-rest-client</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.elasticsearch.client</groupId>
            <artifactId>elasticsearch-rest-client</artifactId>
            <version>${elasticsearch.version}</version>
        </dependency>
        <dependency>
            <groupId>org.elasticsearch</groupId>
            <artifactId>elasticsearch</artifactId>
            <version>${elasticsearch.version}</version>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.10</version>
        </dependency>

        <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>29.0-jre</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.1</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.7.20</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.60</version>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.1.0</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.25</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.22</version>
            <scope>runtime</scope>
        </dependency>

    </dependencies>

