node {
try {
stage('Build') {
sh '''
echo "Building Java project..."
echo "Listing workspace contents:"
ls
cd "Password Protection"
mkdir -p build
javac -d build src/*.java
echo "Build successful"
'''
}
stage('Test') {
    steps {
        sh '''
        echo "Running JUnit tests for File-Encrypter..."
        cd "Password Protection"

        # Download JUnit jar if not exists
        if [ ! -f junit-platform-console-standalone.jar ]; then
            curl -L -o junit-platform-console-standalone.jar https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar
        fi

        # Create test-build directory
        mkdir -p test-build

        # Compile source files including JUnit jar
        javac -cp junit-platform-console-standalone.jar:build -d test-build src/*.java src/*Test.java

        # Run tests
        java -jar junit-platform-console-standalone.jar -cp test-build --scan-class-path --reports-dir test-results
        '''
    }
    post {
        always {
            junit 'Password Protection/test-results/*.xml'
        }
    }
}
stage('Deploy') {
sh '''
echo "Deploying (Packaging) File-Encrypter Application..."
cd "Password Protection"
# Create executable artifact (JAR)
jar cf FileEncrypter.jar -C build .
echo "Deployment successful - Artifact ready"
'''
}
echo "Pipeline executed successfully!"
} catch (Exception e) {
echo "Pipeline failed!"
throw e
}
}
