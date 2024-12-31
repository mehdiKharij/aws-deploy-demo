pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'alibouali/aws-demo:1.0'  // Image Docker mise à jour
        EC2_IP = '13.60.167.33'  // IP publique de votre instance EC2
        USER = 'ec2-user'  // Utilisateur par défaut pour Amazon Linux
        // Contenu de la clé privée SSH sous forme de variable d'environnement
        SSH_PRIVATE_KEY = '''-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAztt+Vms+++dR2pCmeFT81mSwqGXo27NpmV2ASf2MfN/uT8Yg
gYm4wI7sQernQrxSo6Dfnnh/QUlYqN54tyxSbTgpGokUEfU3m4YW0BWi6sqenG6K
suUH8+bKd5Bq9SqtHCV4PXri7k/Ij4IJ3I9RN+bop48A0fwVdWjw4cci0NyXP+fY
4d9OBCxArxjpttS/5qV+PK3d9gAHbdEjVJVkscWOohUhu53sWKaoWBcdttmVxjNa
zcZnBsPX5pAkyQutynCi4QFhZ9SHzLBVA3VKgqQ2RUNUTP37sVvBg8O0TAL4jP8e
fHT6xTLM2a+vIHXdfqZPHnAgnQ/VamV19TBarwIDAQABAoIBAH/QI5nmHj6ryWnR
IusLKEaYZIdIoL7PdpoqqdAN+DZYbvmfpAomPJ/OL7DzIf2cOzubdVCHh6mhVkTR
Yalcm3mcz9jzhhEqgTd5fLMKC2Yj2Ck0LEMpPOa5XbTO6SefPOM9S7RBL+KsLXJu
mQVNEJQH+w09UPZuWhv3wY7f6mU+KhMscFP1yp+afkxhUx53zCyZWN/mfkpEgyak
JeXxWIDpssgecKdwpLs98KhxjXgddIa8HkvS2zCGy437Wo2kxr8Zb0LHgyravB0K
GvMM+kSJvOzG2kYzRUTTUZzyQ7M5RmDC60rXmwV5+267nNOFKpLM4ErCWKbmCvMU
SQKiGWECgYEA/R3VxcI8QZfdNsnpK5dKjY68VHTp3bfKoQx2t3ZJXi6vOnUTqdsS
hFmBIalgJt7tmw0aNoNICgCtJ9pV0oj1R7SEYPPheFr1k7LjUlt/wwPHIVkP6kuw
njM2/gi+IkweA+3s5FsxspykykC45ienEO1Beb1QZsFAdb+FuBjKZPkCgYEA0TbA
sHyEkrrYY6+0lQT06BveQUCH8QPW6obIjvrQSjfeIPtlbPdbxplCGhHD6X5fSYfs
FdBsMBDv6JLQ2wZyzCxAMqA4SbxHln/y95+TodxtlDr19VVpdpPZcQDIJHTefALv
z2ebhi6rAmdBIje5QN3EJglsEsMmcLEPyaberucCgYA7vogP1qn8XYkkfoIf/OTn
BtLjKHlvEQut+dAnu67ToTdRGANdK01ymeHF/UGpyyLQX0ZZqaEeY0x7xKhPOm6S
z0QT0lzc2LNwel/3t4svj7u33lrIVbdJkBMh4RRn6gmHqegpenD/lMO8zYwXHzLq
5uV2g73vkxvQ7zTz4D4dmQKBgEQWrrWBgOAEIUKoP3w0RcR4tWfFKbj9x+dCAGnQ
JRPShN91EfhZtKu42zOCzqDiOP7EVshusZcfHAU0KvbtbVUXnbkcPdV5ik7ny1pd
u/10sNEKM1wp9Q3bZmwJlhmPU41Go2d7z7jm3U8F7cPGIOuEgT7j2CFvE8afSYUW
YujHAoGAYOvGeLBtmHV/eh7qcnCDSGIu9J/bwTKNaSJiGHSTgvpchrnfFQoDdA8q
f5gLWLVd+bF441Bp4ehdj9tJqPekwUPW2dfgTtElT37ZJkv+wxQDaixG4B3jCH66
iHsEQaz+IVkbXEP2pNisw6MBx9862+ybYp1t9p6WyhciUzNx56E=
-----END RSA PRIVATE KEY-----'''
    }

    stages {
        stage('Deploy to EC2') {
            steps {
                script {
                    // Créer un fichier temporaire pour la clé privée
                    writeFile file: 'key.pem', text: env.SSH_PRIVATE_KEY
                    // Fixer les permissions du fichier
                    sh 'chmod 600 key.pem'

                    // Exécution des commandes SSH
                    sh 'ssh -i key.pem -T -o StrictHostKeyChecking=no ec2-user@13.60.167.33 "docker stop deployspring || true"'
                    sh 'ssh -i key.pem -T -o StrictHostKeyChecking=no ec2-user@13.60.167.33 "docker rm deployspring || true"'
                    sh 'ssh -i key.pem -T -o StrictHostKeyChecking=no ec2-user@13.60.167.33 "docker pull alibouali/aws-demo:1.0"'
                    sh 'ssh -i key.pem -T -o StrictHostKeyChecking=no ec2-user@13.60.167.33 "docker run -d --name deployspring -p 80:8080 alibouali/aws-demo:1.0"'

                    // Supprimer le fichier temporaire de la clé privée après l'utilisation
                    sh 'rm -f key.pem'
                }
            }
        }
    }
}

