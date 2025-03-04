# vault-maven-plugin

This Maven plugin supports pull and pushing Maven project properties from secrets stored in [HashiCorp](https://www.hashicorp.com) [Vault](https://www.vaultproject.io/).

## Usage

To include the vault-maven-plugin in your project add the following plugin to your `pom.xml` file:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>io.github.schereradi</groupId>
            <artifactId>vault-maven-plugin</artifactId>
            <version>1.1.3</version>
        </plugin>
    </plugins>
</build>
```

### Pulling Secrets

In order to pull secrets you must add an execution to the plugin.  The following execution will pull secrets from `secret/user` path on the Vault server `https://vault.example.com`.  In particular, this configuration will set the value of the `${project.password}` and `${project.username}` Maven properties to the secrets `${vault.password}` and `${vault.username}` respectively.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>io.github.schereradi</groupId>
            <artifactId>vault-maven-plugin</artifactId>
            <version>1.1.3</version>
            <executions>
                <execution>
                    <id>pull</id>
                    <phase>initialize</phase>
                    <goals>
                        <goal>pull</goal>
                    </goals>
                    <configuration>
                        <servers>
                            <server>
                                <url>https://vault.example.com</url>
                                <token>bf6ba314-47f1-4b9d-ab87-2b8e53fc640f</token>
                                <paths>
                                    <path>
                                        <name>secret/user</name>
                                        <mappings>
                                            <mapping>
                                                <key>vault.password</key>
                                                <property>project.password</property>
                                            </mapping>
                                            <mapping>
                                                <key>vault.username</key>
                                                <property>project.username</property>
                                            </mapping>
                                        </mappings>
                                    </path>
                                </paths>
                            </server>
                        </servers>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

Note that the execution will fail if a specified secret key does not exist and that an existing project property will be overwritten.

### Pushing Secrets

In order to pull secrets you must add an execution to the plugin.  The following execution will pull secrets from `secret/user` path on the Vault server `https://vault.example.com`.  In particular, this configuration will set the value of the `${project.password}` and `${project.username}` Maven properties to the secrets `${vault.password}` and `${vault.username}` respectively.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>io.github.schereradi</groupId>
            <artifactId>vault-maven-plugin</artifactId>
            <version>1.1.3</version>
            <executions>
                <execution>
                    <id>push</id>
                    <phase>verify</phase>
                    <goals>
                        <goal>push</goal>
                    </goals>
                    <configuration>
                        <servers>
                            <server>
                                <url>https://vault.example.com</url>
                                <token>bf6ba314-47f1-4b9d-ab87-2b8e53fc640f</token>
                                <paths>
                                    <path>
                                        <name>secret/user</name>
                                        <mappings>
                                            <mapping>
                                                <key>vault.password</key>
                                                <property>project.password</property>
                                            </mapping>
                                            <mapping>
                                                <key>vault.username</key>
                                                <property>project.username</property>
                                            </mapping>
                                        </mappings>
                                    </path>
                                </paths>
                            </server>
                        </servers>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

Note that the execution will fail if a specified project property does not exist and that an existing secret value will be overwritten.

## Building

This build uses standard Maven build commands but assumes that the following are installed and configured locally:

1) Java (1.8 or greater)
1) Maven (3.0 or greater)
1) Docker

## Contributing

1. Fork it
1. Create your feature branch (`git checkout -b my-new-feature`)
1. Commit your changes (`git commit -am 'Add some feature'`)
1. Push to the branch (`git push origin my-new-feature`)
1. Create new Pull Request
