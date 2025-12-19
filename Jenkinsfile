pipeline{
    agent any
    
    stages {
        
        stage('Prueba echo')
        {
            steps {
                //Realizar un echo//
               sh 'echo Este es mi primer pipeline'
                }
        }
        
        stage('Get Code')
        {
            steps {
                //Descargar códico desde mi repositorio//
                git 'https://github.com/ramirodhen/helloworld.git'
                //Verificación de descarga de código//
                sh 'ls -la'
                //Mostrar mi espacio de trabajo//
                sh 'echo $WORKSPACE'
                }
        }
        
        stage('Build')
        {
            steps {
                //Etapa de complilación simulada//
                sh 'echo Nada que compilar'
            }  
        }
        stage('Tests'){
            parallel {
                stage('Unit')
                {
                    steps {
                        //Pruebas unitarias con pytest//
                        sh '''
                        export PYTHONPATH=.
                        pytest --junitxml=resultado-unit.xml test/unit
                        '''
                    }
                }
                
                stage('Service')
                {
                    steps {
                        //Levantamos Entorno de pruebas//
                        sh '''
                        set -e
                        export FLASK_APP=app.api:api_application
                        flask run &
                        java -jar /opt/wiremock/wiremock-standalone-3.13.2.jar --port 9090 --root-dir "$WORKSPACE/test/wiremock" &
                        sleep 5
                        echo "fin sleep"
                        # Ejecutamos 
                        pytest --junitxml=resultado-service.xml test/rest
                        '''
                         }
                }
            }
        }
        stage ('Resultado')
        {
            steps {
            junit 'resultado*.xml'
                }
        }
     }
}
