pipeline {
	
	agent none
	
    options{
        timestamps()
        ansiColor('xterm')
    }
	
	environment {
        ContainerName = "${JOB_NAME}-${BUILD_NUMBER}"
    }

	stages {
	
		stage('Check Out') {
			agent {label 'gebldrh1'}
			
			steps {
		        sh 'cd ~/jenkins/workspace'
                sh 'echo "UID:" $UID'
		    	checkout([$class: 'GitSCM', branches: [[name: 'origin/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'WipeWorkspace'], [$class: 'PathRestriction', excludedRegions: '', includedRegions: '''wmx-fetch-api/.* wmx-javascript-zuma/.* Client/.*''']], gitTool: 'linux_git', submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'cb460de8-37ae-4062-b1de-5dea3f247823', url: 'git@devtopia.esri.com:/WebGIS/workflow-manager.git']]])
			}
		}
		
        stage('Docker Container') {
			agent {
				docker {
					label 'gebldrh1'
					image 'ogbuild-docker.olympus.esri.com/jenkins-node-gulp-chromium'
					args  '-e JAVA_TOOL_OPTIONS="-Dfile.encoding=UTF-8" -e LC_ALL="en_US.UTF-8" --privileged --name "${ContainerName}" -u 16841635:16777729 -v /home/AVWORLD/txbuild/jenkins/caches:/home/AVWORLD/txbuild/jenkins/caches -v /mnt:/mnt -v /home/AVWORLD/txbuild/jenkins/tools:/home/AVWORLD/txbuild/jenkins/tools --entrypoint=/var/txbuildStart.sh'
				}
			}
			
			stages {
			
				stage("Build Step 1") {
					steps {
						sh """
							HOME=/home/txbuild
							PATH=$PATH:/bin
							cat /etc/alpine-release
							npm config set cache /home/AVWORLD/txbuild/jenkins/caches/wmx-fetch-api/.npm
							npm config set color false
							npm config set progress false
							reset
							cd /home/AVWORLD/txbuild/jenkins/workspace/WMX_Zuma_Javascript_Pipeline_Build/Client/wmx-fetch-api
							pwd
							#ls /net/ps-RED-STOR3
							#ls /home/AVWORLD/txbuild/jenkins/workspace/WMX_Zuma_Javascript_Pipeline_Build/wmx-fetch-api
							npm install
							npm pack
							
							
							cd /home/AVWORLD/txbuild/jenkins/workspace/WMX_Zuma_Javascript_Pipeline_Build/Client/wmx-javascript-zuma
							
							#JSPM_GLOBAL_PATH=/home/AVWORLD/txbuild/jenkins/caches/WMX_Zuma_Javascript_Pipeline_Build && export JSPM_GLOBAL_PATH 
							npm config set cache /home/AVWORLD/txbuild/jenkins/caches/WMX_Zuma_Javascript_Pipeline_Build/.npm
							npm config set color false
							npm config set progress false
							reset
							
							
							#whoami
							#echo $JOB_NAME
							#echo "Hello World!" > /home/AVWORLD/txbuild/jenkins/workspace/WMX_Zuma_Javascript_Pipeline_Build/myTest.txt
							#echo $HOME
							#uname -n
							#uname -a
							#cat /etc/redhat-release
							#cat /etc/system-release
							#pwd
							#ls --color=never -la
							#mkdir .npm
							#ls --color=never -la /home
							node -v
							npm -v
							#/bin/cat
							# using https instead of git to bypass the new npm build issue
							#git config --global url."https://github.com/".insteadOf git@github.com:
							#git config --global url."https://".insteadOf git://
							
							git --version
							#git config --global url."https://".insteadOf git://
							#npm install ../wmx-fetch-api
							#rm -rf /home/AVWORLD/txbuild/jenkins/workspace/WMX_Zuma_Javascript_Pipeline_Build/wmx-fetch-api
							#cp -r /home/AVWORLD/txbuild/jenkins/workspace/wmx-fetch-api/ /home/AVWORLD/txbuild/jenkins/workspace/WMX_Zuma_Javascript_Pipeline_Build/wmx-fetch-api/
							
							#npm --no-color --unsafe-perm install
							npm --no-color install
							npm prune
							#gulp -v
							npm run extractStrings 
							mkdir -p "$WORKSPACE/Client/wmx-javascript-zuma/src/locale/" && cp "$WORKSPACE/Client/wmx-javascript-zuma/.tmp/compiled/messages.xlf" "$WORKSPACE/Client/wmx-javascript-zuma/src/locale/"
							
							#mv ./node_modules/dojo-dstore ./node_modules/dstore
							#ls -la ./node_modules/dstore
							#ls -la ./node_modules/dojo-dstore
							#./node_modules/.bin/jspm registry create bower jspm-bower-endpoint -y 
							#./node_modules/.bin/jspm install 
							#jspm install -y
							./node_modules/.bin/gulp clean 
							npm --no-color run build
							
							#run unit-test
							npm run test
							#./node_modules/.bin/gulp test
							rm -rf ${WORKSPACE}/Client/wmx-javascript-zuma/wmx-test-logs/
							mkdir -p "${WORKSPACE}/Client/wmx-javascript-zuma/wmx-test-logs/" && cp ${WORKSPACE}/Client/wmx-javascript-zuma/output/test-reports/*.xml ${WORKSPACE}/Client/wmx-javascript-zuma/wmx-test-logs/
							cd ${WORKSPACE}/Client/wmx-javascript-zuma/wmx-test-logs/
							mv *.xml test-report.xml
							
							#umount /home/AVWORLD/txbuild/jenkins/workspace/wmx-fetch-api
						"""
					}
				}
				stage('Build Step 2') {
					steps {
						sh """
							PATH=$PATH:/bin
							rm -rf /home/AVWORLD/txbuild/jenkins/workspace/WMX_Zuma_Javascript_Pipeline_Build/Client/wmx-javascript-zuma/staging/
							cp -r /home/AVWORLD/txbuild/jenkins/workspace/WMX_Zuma_Javascript_Pipeline_Build/Client/wmx-javascript-zuma/dist/ /home/AVWORLD/txbuild/jenkins/workspace/WMX_Zuma_Javascript_Pipeline_Build/Client/wmx-javascript-zuma/staging/
							cd ${WORKSPACE}/Client/wmx-fetch-api
							tar -zvcf wmx-fetch-api-node_modules.tar.gz ./node_modules/
							rm -rf /mnt/Ent_Sol/JTX/Builds/Archives/Zuma/JavaScript/node_modules/wmx-fetch-api-node_modules.tar.gz
							cp -rf wmx-fetch-api-node_modules.tar.gz /mnt/Ent_Sol/JTX/Builds/Archives/Zuma/JavaScript/node_modules/
							rm -rf ../wmx-fetch-api-build-files.tar.gz
							# --warning=no-file-changed 
							tar --exclude='./node_modules' --exclude='./wmx-fetch-api-node_modules.tar.gz' --exclude='./typings' --exclude='./wmx-fetch-api-build-files.tar.gz' -zcvf ../wmx-fetch-api-build-files.tar.gz .
							echo "test"
							echo "test1 $BUILD_NUMBER"
							mkdir -p /mnt/Ent_Sol/JTX/Builds/Archives/Zuma/JavaScript/master/$BUILD_NUMBER
							cp -rf ../wmx-fetch-api-build-files.tar.gz /mnt/Ent_Sol/JTX/Builds/Archives/Zuma/JavaScript/master/$BUILD_NUMBER
							cd ${WORKSPACE}/Client/wmx-javascript-zuma
							tar -zvcf wmx-javascript-zuma-node_modules.tar.gz ./node_modules/
							rm -rf /mnt/Ent_Sol/JTX/Builds/Archives/Zuma/JavaScript/node_modules/wmx-javascript-zuma-node_modules.tar.gz
							cp -rf wmx-javascript-zuma-node_modules.tar.gz /mnt/Ent_Sol/JTX/Builds/Archives/Zuma/JavaScript/node_modules/
							rm -rf ../wmx-javascript-zuma-build-files.tar.gz
							#--warning=no-file-changed 
							tar --exclude='./node_modules' --exclude='./wmx-javascript-zuma-node_modules.tar.gz' --exclude='./wmx-javascript-zuma-build-files.tar.gz' -zcvf ../wmx-javascript-zuma-build-files.tar.gz .
							mkdir -p /mnt/Ent_Sol/JTX/Builds/Archives/Zuma/JavaScript/master/$BUILD_NUMBER
							cp -rf ../wmx-javascript-zuma-build-files.tar.gz /mnt/Ent_Sol/JTX/Builds/Archives/Zuma/JavaScript/master/$BUILD_NUMBER
							cd ${WORKSPACE}/Client/wmx-gojs
							rm -rf ../wmx-gojs-build-files.tar.gz
							#--warning=no-file-changed 
							tar --exclude='./node_modules' --exclude='./wmx-gojs-node_modules.tar.gz' --exclude='./typings' --exclude='./wmx-gojs-build-files.tar.gz' -zcvf ../wmx-gojs-build-files.tar.gz .
							mkdir -p /mnt/Ent_Sol/JTX/Builds/Archives/Zuma/JavaScript/master/$BUILD_NUMBER
							cp -rf ../wmx-gojs-build-files.tar.gz /mnt/Ent_Sol/JTX/Builds/Archives/Zuma/JavaScript/master/$BUILD_NUMBER
						"""
					}
				}
				
				stage('Publish Test Results') {
					steps {
						junit 'Client/wmx-javascript-zuma/wmx-test-logs/*.xml'
					}
				}
			}
        }
	}
	post { 
        success { 
			node('gebldrh1')
			{
				//build job: 'WMX_Zuma_Javascript_LTK_Build', parameters: [string(name: 'WMX_BUILDNO', value: ''), string(name: 'config', value: ''), string(name: 'Pseudo_Translation', value: 'true'), string(name: 'langBuild', value: 'es,it,ru,tr,zh-CN')]
			}
		}
    }
}