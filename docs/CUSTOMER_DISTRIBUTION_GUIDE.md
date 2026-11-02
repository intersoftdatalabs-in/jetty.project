# Eclipse Jetty 9.4 Extended Support Distribution Guide (Version 9.4.65)

This guide provides instructions for customers running legacy Java 8 applications who require continued support and vulnerability remediation for Eclipse Jetty 9.4.

This distribution targets **Jetty 9.4.65** and is hosted on GitHub Packages and GitHub Releases by Intersoft Data Labs.

---

## 1. Vulnerability Fixes Included in 9.4.65

| CVE ID | Severity | Component | Summary |
| :--- | :--- | :--- | :--- |
| **CVE-2025-11143** | Medium | `jetty-http` | Path normalization in `HttpURI` resolving ambiguous encoded URI segments and directory traversal bypasses. |
| **CVE-2026-10050** | High | `jetty-client`, `jetty-security` | Lossy ISO-8859-1 conversion in Digest Authentication leading to authentication bypass via unmappable characters (CWE-173). UTF-8 support added per RFC 7616. |
| **CVE-2026-18659** | High | `jetty-security` | Method omission constraint matching substring flaw in `ConstraintSecurityHandler` (`!entry.getKey().contains(httpMethod)`). |
| **CVE-2026-18662** | Medium | `jetty-security` | Method omission constraint combining bug in `ConstraintSecurityHandler` where consecutive omission definitions overwrote prior permissions. |
| **CVE-2026-18660** | Medium | `jetty-proxy` | Case-sensitive hostname checking in `ConnectHandler` and `AbstractProxyServlet` allowing bypass of whitelist/blacklist proxy rules. |
| **CVE-2026-6790** | Medium | `jetty-server` | URI Authority mismatch with `Host` header in `HttpChannel` violating RFC 9112 §3.2.1 request routing. |
| **CVE-2026-2332** | Medium | `jetty-http` | Chunk extension parameter quote parsing flaw in `HttpParser` allowing request smuggling via quoted strings. |
| **CVE-2026-19203** | Medium | `jetty-http` | Unclosed quotes or embedded CRLF characters inside chunk parameters in `HttpParser`. |

---

## 2. Java Runtime Compatibility

- **Java Version:** Java 8 (OpenJDK 8 or Oracle JDK 8) minimum.
- **Bytecode Level:** Java 8 (`v52.0`) verified across all jars.

---

## 3. Consuming Maven Artifacts via GitHub Packages

The Maven artifacts for Jetty `9.4.65` are hosted on GitHub Packages under the repository `intersoftdatalabs-in/jetty.project`.

### 3.1 Maven Configuration

Add the repository to your `pom.xml`:

```xml
<repositories>
  <repository>
    <id>github-intersoft-jetty</id>
    <name>Intersoft Jetty Fork</name>
    <url>https://maven.pkg.github.com/intersoftdatalabs-in/jetty.project</url>
    <snapshots>
      <enabled>false</enabled>
    </snapshots>
  </repository>
</repositories>
```

Add the dependencies using version `9.4.65`:

```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.eclipse.jetty</groupId>
      <artifactId>jetty-bom</artifactId>
      <version>9.4.65</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>

<dependencies>
  <dependency>
    <groupId>org.eclipse.jetty</groupId>
    <artifactId>jetty-server</artifactId>
    <version>9.4.65</version>
  </dependency>
  <dependency>
    <groupId>org.eclipse.jetty</groupId>
    <artifactId>jetty-servlet</artifactId>
    <version>9.4.65</version>
  </dependency>
</dependencies>
```

### 3.2 Authentication (`~/.m2/settings.xml`)

GitHub Packages requires authenticated access to download packages:

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 https://maven.apache.org/xsd/settings-1.0.0.xsd">
  <servers>
    <server>
      <id>github-intersoft-jetty</id>
      <username>YOUR_GITHUB_USERNAME</username>
      <password>YOUR_GITHUB_PERSONAL_ACCESS_TOKEN</password>
    </server>
  </servers>
</settings>
```
*Note: Your Personal Access Token (PAT) requires the `read:packages` scope.*

---

## 4. Gradle Configuration

```groovy
repositories {
    mavenCentral()
    maven {
        url = uri("https://maven.pkg.github.com/intersoftdatalabs-in/jetty.project")
        credentials {
            username = project.findProperty("gpr.user") ?: System.getenv("GITHUB_ACTOR")
            password = project.findProperty("gpr.key") ?: System.getenv("GITHUB_TOKEN")
        }
    }
}

dependencies {
    implementation 'org.eclipse.jetty:jetty-server:9.4.65'
    implementation 'org.eclipse.jetty:jetty-servlet:9.4.65'
}
```

---

## 5. Standalone Server Distribution Archives

For customers running standalone Jetty instances, binary distribution archives are provided:

- `jetty-distribution-9.4.65.tar.gz`
- `jetty-distribution-9.4.65.zip`

### Verification:

```bash
sha256sum -c sha256sums.txt
```

### Starting the Server:

```bash
tar -xzf jetty-distribution-9.4.65.tar.gz
cd jetty-distribution-9.4.65
java -jar start.jar
```
