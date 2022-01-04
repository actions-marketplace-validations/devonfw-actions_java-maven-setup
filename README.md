# GitHub Composite Action for very basic Java / Maven / Git basics

Features and their parameters: 

| Feature  | Parameters to be set/for control  | Reference | Comment |
|---|---|---|---|
| Caching of maven repository (optional)  | maven-cache-key (actions/cache>key)<br />maven-cache-restore-key (actions/cache>restore-key)  | [actions/cache@v2](https://github.com/actions/cache/blob/main/examples.md#java---maven)  | |
| Switching Java version (optional)  | java-version  | [actions/setup-java@v1](https://github.com/actions/setup-java#supported-version-syntax)  | |
| Setting up GPG signing (optional)  | GPG_PRIVATE_KEY<br />GPG_PASSPHRASE  | [crazy-max/ghaction-import-gpg@v4](https://github.com/devonfw-actions/java-maven-setup/blob/main/action.yml#L55)  |   |
| Prepare commit to protected branches (optional)  | BUILD_USER<br />BUILD_USER_PASSWD<br />BUILD_USER_EMAIL  |   | To be able to finally push afterwards to the repository. You have to disable default github actions authentication header. See an example [here](https://github.com/devonfw/cobigen/blob/eb8519c4dd69b307dd1e669eeab0bfba59c1c1a1/release.sh#L171) |
| Setting up allow long git paths on windows  | auto applied  |   |   |
| Use GNU tar instead BSD tar on windows  | auto applied  |   | Workaround as described [here](https://github.com/actions/cache/issues/576#issuecomment-830796954) |

## Usage Examples

### for simple setup and caching
```
steps:
  - uses: devonfw-actions/java-maven-setup@v1
    with:
      maven-cache-key: ${{ runner.os }}-${{ matrix.javaVersion }}-maven-${{ github.sha }}
      maven-cache-restore-key: cobigen-dep-${{ hashFiles('**/pom.xml') }}
      java-version: ${{ matrix.javaVersion }}
```

### for release script setup
```
steps:
  - uses: devonfw-actions/java-maven-setup@v1
    with:
      maven-cache-key: cobigen-dep-${{ hashFiles('**/pom.xml') }}
      GPG_PRIVATE_KEY: ${{ secrets.GPG_PRIVATE_KEY }}
      GPG_PASSPHRASE: ${{ secrets.GPG_PASSPHRASE }}
      BUILD_USER: ${{ secrets.BUILD_USER }}
      BUILD_USER_PASSWD: ${{ secrets.BUILD_USER_PASSWD }}
      BUILD_USER_EMAIL: ${{ secrets.BUILD_USER_EMAIL }}
```
