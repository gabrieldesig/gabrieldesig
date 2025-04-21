cat <<EOF > comandos.txt
mkdir tarefa-git
cd tarefa-git
git init

echo "Este é o conteúdo do arquivo um." > arquivo1.txt
echo "Aqui temos outro conteúdo, no segundo arquivo." > arquivo2.txt
echo "O terceiro arquivo também tem seu próprio conteúdo." > arquivo3.txt

git add arquivo1.txt arquivo2.txt arquivo3.txt
git commit -m "Commit inicial com três arquivos de texto"

echo "Adicionando uma nova linha ao arquivo um." >> arquivo1.txt
echo "Arquivo dois recebeu esta nova linha." >> arquivo2.txt

git status
git diff

git add arquivo1.txt arquivo2.txt
git commit -m "Alterações nos arquivos 1 e 2"

git log
git log --oneline
git log --oneline --graph --all
git log -p

git revert HEAD

echo "Esta linha será armazenada temporariamente." >> arquivo3.txt
git stash
git checkout -b nova-linha-desenvolvimento
git checkout main
git stash pop

git stash list
git reflog

git log --reverse --oneline
git tag -a v1.0 -m "Versão estável inicial" abc1234
git push origin v1.0
EOF
