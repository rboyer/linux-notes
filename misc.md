Create a "correct horse battery staple" password:

     echo $(grep '^[a-z]\{5,10\}$' /usr/share/dict/words | shuf | head -n 4)
