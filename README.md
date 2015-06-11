# MBG-CommentGenerator
用于 Mybatis Generator 根据数据库生成对应的中文注释
------
Mybatis Generator 配置 见 resources/demo/generatorConfig.xml

*注意：*  
```
  <!-- 这里配置成修改过的类 -->
	<commentGenerator type="com.xrkj.utils.mbg.MyCommentGenerator"><!-- 配置成字节的类，生成中文注释 -->
		<property name="suppressAllComments" value="true" /><!-- 生成注释 -->
		<property name="suppressDate" value="true" /><!-- 生成的注释包含时间戳 -->
	</commentGenerator>
```
Maven 配置
-----
```
  <plugin>
		<groupId>org.mybatis.generator</groupId>
		<artifactId>mybatis-generator-maven-plugin</artifactId>
		<version>1.3.2</version>
		<configuration>
			<verbose>true</verbose>
			<overwrite>true</overwrite>
		</configuration>

		<dependencies>
			<!-- 数据库驱动 -->
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
				<version>${mysql.version}</version>
			</dependency>
			<dependency>
				<groupId>com.xrkj.app</groupId>
				<artifactId>MBG-CommentGenerator</artifactId>
				<version>0.0.1</version>
			</dependency>
			<dependency>
				<groupId>org.mybatis</groupId>
				<artifactId>mybatis</artifactId>
				<version>3.2.8</version>
			</dependency>
		</dependencies>
	</plugin>
```
--
如下：<br>
MBG插件将会绑定到Maven构建的 generate-sources 阶段
```
<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.3</version>
				<configuration>
					<source>1.7</source>
					<target>1.7</target>
					<encoding>UTF-8</encoding>
				</configuration>
			</plugin>

			<plugin>
				<groupId>org.mybatis.generator</groupId>
				<artifactId>mybatis-generator-maven-plugin</artifactId>
				<version>1.3.2</version>
				<configuration>
					<verbose>true</verbose>
					<overwrite>true</overwrite>
				</configuration>

				<dependencies>
					<!-- 数据库驱动 -->
					<dependency>
						<groupId>mysql</groupId>
						<artifactId>mysql-connector-java</artifactId>
						<version>${mysql.version}</version>
					</dependency>
					<dependency>
						<groupId>com.xrkj.app</groupId>
						<artifactId>MBG-CommentGenerator</artifactId>
						<version>0.0.1</version>
					</dependency>
					<dependency>
						<groupId>org.mybatis</groupId>
						<artifactId>mybatis</artifactId>
						<version>3.2.8</version>
					</dependency>

				</dependencies>

				<executions>
					<!-- 由于M2e不支持这个goal，会报错，忽略这个goal就好了 -->
					<execution>
						<id>Generate MyBatis Artifacts</id>
						<goals>
							<goal>generate</goal>
						</goals>
					</execution>
				</executions>

			</plugin>

		</plugins>
		<!-- 解决办法把下面这段配置添加到与plugins平级目录中即可解决： -->
		<pluginManagement>
			<plugins>
				<plugin>
					<groupId>org.eclipse.m2e</groupId>
					<artifactId>lifecycle-mapping</artifactId>
					<version>1.0.0</version>
					<configuration>
						<lifecycleMappingMetadata>
							<pluginExecutions>
								<pluginExecution>
									<pluginExecutionFilter>
										<groupId>org.mybatis.generator</groupId>
										<artifactId>mybatis-generator-maven-plugin</artifactId>
										<versionRange>[1.3.2,)</versionRange>
										<goals>
											<goal>generate</goal>
										</goals>
									</pluginExecutionFilter>
									<action>
										<ignore />
									</action>
								</pluginExecution>
							</pluginExecutions>
						</lifecycleMappingMetadata>
					</configuration>
				</plugin>
			</plugins>
		</pluginManagement>
	</build>
