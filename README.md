Good night fellas - after fix
Игнорируется всё, что находится в папке .terraform
*/.terraform/*

Игнорятся все файлы с расширением tfstate, а так же все файлы в названии которых есть .tfstate.
*.tfstate
*.tfstate.*

Игнорятся - файл с названием crash.log, и файлы название которых начинается на crash. и расширением log
crash.log
crash.*.log

Игнорятся все файлы со следующими расширениями
*.tfvars
*.tfvars.json

Игнорятся файлы с названиями override.tf и override.tf.json, а так же все файлы, которые имеют окончание названия и расширение _override.tf и _override.tf.json
override.tf
override.tf.json
*_override.tf
*_override.tf.json

Игнорятся файлы с названиями
.terraformrc
terraform.rc
