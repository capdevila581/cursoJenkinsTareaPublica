//Plantilla de PIPELINE
//Al cambiar la version solo se recarga la configuracion del proyecto.. pero no se ejecutaran las tareas
VERSION_DEL_PIPELINE = "1.1"
PARAMETROS_DE_PIPELINE = [
        //choice( name: 'ENTORNO', description: 'Elija un entorno', choices: [ 'Desarrollo','Preproducción','Producción' ] ) ,
        booleanParam( name: 'GENERAR_ERROR', description: 'Variable boolean', defaultValue:false) ,
        string( name: 'DEMORA', description: 'Variable string', defaultValue: '')     ,
        string( name: 'CONTENIDO', description: 'Variable string', defaultValue: '')    ,
        string( name: 'FICHERO', description: 'FICHERO que generamos', defaultValue: '')    
    ]
TRIGGERS_DE_PIPELINE = [
        //cron("* * * * *")                                                         //Ejecutar job cada minuto
        //pollSCM("* * * * *")                                                      //Ejecutar cuando cambie algo en GIT
        //upstream(upstreamProjects: 'PROYECTO_DISPARADOR", threshold: 'FAILURE')   //Ejecutar tras otro proyecto
    ]

//No tocar este proceso
if (this.params.getOrDefault('VERSION_DEL_PIPELINE',"-1") != VERSION_DEL_PIPELINE) {
    stage('Configuración del JOB'){
        crearConfiguracion()
    }
    return
}

node {
    //Obtener la ultima version en git
    //checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'b1e0b88c-7ac7-4c67-8ed3-6d289440cc83', url: 'https://github.com/capdevila581/cursoJenkinsWebapp.git']]])
    // checkout scm // En el caso que este configurado el git en el JOB de Jenkins
    
    //Aqui empiezan las tareas propias de mi pipeline
    try{  

        stage ("Hacer cosas") {
            sh "sleep ${DEMORA}"
        }
        stage ("Hacer cosas peligrosas") {
            try{
                if(this.params.GENERAR_ERROR){
                sh "exit 1"
            }
            }catch(exc){
                echo 'ERROR' 
                throw exc
            }
            echo 'Pues no ha sido para tanto' 
        }
        stage ("Generar fichero") {
            sh "echo '${CONTENIDO}' > ${FICHERO}"
        }
        stage ("Guardar fichero") {
            sh "sleep ${DEMORA}"
        }
        
    }finally{
        stage ("Limpieza del workspace") {
            docker.image('maven:3.8.3-openjdk-8').inside {
                sh 'mvn clean'
            }    
            try{
                sh "docker rm ${env.ID_CONTENEDOR} -f"
            }
            catch(exc){
                echo 'Parece que no hemos podido borrar el contenedor'
            }
        }
    }
    

}

//Configuracion
def crearConfiguracion() {
    properties(
        [
            parameters(
                [
                    //string(defaultValue:'0' , name: 'CODIGO_SALIDA')  
                    choice( name: 'VERSION_DEL_PIPELINE',
                            description: 'Version del pipeline instalada',
                            choices: [ VERSION_DEL_PIPELINE ] )
                ] + PARAMETROS_DE_PIPELINE
            ),
            pipelineTriggers( TRIGGERS_DE_PIPELINE )
        ]    
    )
}

