
VERSION = 0.4.15
FILES = Makefile walon.aff wa_affix.dat wa_phonet.dat wa.dat wa.multi \
	ChangeLog LIJHOZMU README LGPL TODO \
	firefox.tar.bz2
FILES_DOS = make.bat install.bat
EXTRA = ER_viebes Aa Bb HAUST Cc

# djivêye des rîles a n' *nén* copyî
WA_LIDJE=':wa-ae-a\|:wa-aa-a\|:wa-aajh-a\|:wa-aacion-a\|:wa-ea-ya\|:wa-ey-iy\|:wa-oen-w\|:wa-oe-w\|:wa-oi-o\|:wa-oi-wa\|:wa-sch-[sc]\|:wa-sh-ch\|:wa-h-_\|:wa-jh-j\|:wa-xh-ch\|:wa-o-ou\|:wa-owe-uwe\|:wa-u-ou'
WA_NAMEUR=':wa-ae-e\|:wa-aa-a\|:wa-aajh-a\|:wa-aacion-o\|:wa-ea-e\|:wa-ey-ey\|:wa-oen-o\|:wa-oe-e\|:wa-oi-o\|:wa-oi-we\|:wa-sch-[sh]\|:wa-sh-ss\|:wa-h-h\|:wa-jh-h\|:wa-xh-h\|:wa-o-o\>\|:wa-u-ou'
WA_HOUFALIJHE=':wa-ae-e\|:wa-aa-o\|:wa-aajh-o\|:wa-aacion-o\|:wa-ea-ya\|:wa-ey-iy\|:wa-oen-w\|:wa-oe-w\|:wa-oi-o\|:wa-oi-wa\|:wa-sch-[sh]\|:wa-sh-ch\|:wa-h-_\|:wa-jh-j\|:wa-xh-h\|:wa-o-ou\|:wa-owe-uwe\|:wa-u-ou'
WA_TCHALERWE=':wa-ae-e\|:wa-aa-a\|:wa-aajh-a\|:wa-aacion-o\|:wa-ea-e\|:wa-ey-ey\|:wa-oen-o\|:wa-oe-e\|:wa-oi-we\|:wa-oi-wa\|:wa-sch-[hc]\|:wa-sh-ch\|:wa-h-h\|:wa-jh-h\|:wa-xh-h\|:wa-o-o\>\|:wa-u-ou'
WA_NIVELE=':wa-ae-e\|:wa-aa-a\|:wa-aajh-a\|:wa-aacion-o\|:wa-ea-e\|:wa-ey-ey\|:wa-oen-o\|:wa-oe-e\|:wa-oi-we\|:wa-oi-o\|:wa-sch-[hc]\|:wa-sh-ss\|:wa-h-h\|:wa-jh-h\|:wa-xh-h\|:wa-o-o\>\|:wa-u-ou'

# walon.munch generates a too big file...
#all:	walon.tot
#all:	walon.hashed
#all:	walon.munched
all:	aspell

walon.tot:	walon.hashed

dj:	walon.aff
	rm -f dj.hash dj.lst
	grep -v '^#' Aa Bb Cc motsrif/* mots2/* |cut -d: -f2- |cut -d'	' -f1|tr ' ' '\n'|sort -u > dj.lst
	buildhash dj.lst walon.aff dj.hash

walon_lst:
	if [ ! -r walon.lst -o -n "`find mots* -type f -newer walon.lst`" ] ; \
	then \
	  grep -v '^#' mots*/* ${EXTRA} |cut -d: -f2- |cut -d'	' -f1|\
	  tr ' ' '\n'|(LC_ALL=C sort -u) | grep -v '^$$' > walon.lst ; \
	fi

wa_lst: walon_lst
	# i fåt côper åzès apostrofes+loyeure pol moumint, aspell ni sait nén
	# prinde des mots avou zeles ambedeus
	if [ ! -r wa.lst -o wa_affix.dat -nt wa.lst -o walon.lst -nt wa.lst ] ; \
	then \
	  cat walon.lst | \
	  LC_ALL=C sed "s/'-.*$$/'/" | \
	  aspell expand --encoding=iso8859-1 --lang=wa --local-data-dir=. |\
	  tr ' ' '\n' > wa.lst ; \
	fi

walon.hashed:	walon.aff walon_lst
	buildhash walon.lst walon.aff walon.hash

walon.munched:	walon.aff walon_lst
	rm -f walon.hash walon.txt 
	munchlist -v -l walon.aff walon.lst > walon.txt
	buildhash walon.txt walon.aff walon.hash

clean:
	rm -f core *.stat *.cnt *.txt wa@*.dat wa@*.rws wa*.dic
	if [ -d aspell-wa-${VERSION}/ ]; then rm -rf aspell-wa-${VERSION}/ ; fi

distclean: clean
	rm -f *.lst

dist:	dist.tar.bz2

distdir: distclean
	mkdir aspell-wa-${VERSION}
	cp -var mots* ${EXTRA} aspell-wa-${VERSION}/
	cp -v ${FILES} aspell-wa-${VERSION}/

distdir_dos: wa_lst clean
	mkdir aspell-wa-${VERSION}
	cp -var wa.lst ${EXTRA} aspell-wa-${VERSION}/
	cp -v ${FILES} aspell-wa-${VERSION}/
	cp -v ${FILES_DOS} aspell-wa-${VERSION}/

dist.tar.bz2: distdir
	tar jcvf aspell-wa-${VERSION}.tar.bz2 aspell-wa-${VERSION}/
	rm -rf aspell-wa-${VERSION}/

dist.tar.gz: distdir
	tar zcvf aspell-wa-${VERSION}.tar.gz aspell-wa-${VERSION}/
	rm -rf aspell-wa-${VERSION}/

dist.zip: distdir_dos
	zip -r aspell-wa-${VERSION}.zip aspell-wa-${VERSION}/
	rm -rf aspell-wa-${VERSION}/

ispell: walon.hashed

aspell: wa_lst
	rm -f wa.rws
	aspell --local-data-dir=. --lang=wa create master ./wa.rws < wa.lst

aspell@lj: wa_lst
	rm -f wa@lj.rws
	grep -v ${WA_LIDJE} wa_phonet.dat > wa@lj_phonet.dat
	cat wa.dat | sed 's/\<wa\>/wa@lj/' > wa@lj.dat
	[ -r wa@lj_affix.dat ] || ln -s wa_affix.dat wa@lj_affix.dat
	aspell --local-data-dir=. --lang=wa@lj create master ./wa@lj.rws < wa.lst

aspell@na: wa_lst
	rm -f wa@na.rws
	grep -v ${WA_NAMEUR} wa_phonet.dat > wa@na_phonet.dat
	cat wa.dat | sed 's/\<wa\>/wa@na/' > wa@na.dat
	[ -r wa@na_affix.dat ] || ln -s wa_affix.dat wa@na_affix.dat
	aspell --local-data-dir=. --lang=wa@na create master ./wa@na.rws < wa.lst

aspell@ch: wa_lst
	rm -f wa@ch.rws
	grep -v ${WA_TCHALERWE} wa_phonet.dat > wa@ch_phonet.dat
	cat wa.dat | sed 's/\<wa\>/wa@ch/' > wa@ch.dat
	[ -r wa@ch_affix.dat ] || ln -s wa_affix.dat wa@ch_affix.dat
	aspell --local-data-dir=. --lang=wa@ch create master ./wa@ch.rws < wa.lst

aspell@ni: wa_lst
	rm -f wa@ni.rws
	grep -v ${WA_NIVELE} wa_phonet.dat > wa@ni_phonet.dat
	cat wa.dat | sed 's/\<wa\>/wa@ni/' > wa@ni.dat
	[ -r wa@ni_affix.dat ] || ln -s wa_affix.dat wa@ni_affix.dat
	aspell --local-data-dir=. --lang=wa@ni create master ./wa@ni.rws < wa.lst

myspell: walon_lst
	wc -l walon.lst > wa.dic
	cat walon.lst >> wa.dic
	# myspell is unable to handle comments and tabs :-(
	grep -v '^#' wa_affix.dat | sed 's/	/    /g' > wa.aff

wa_dic: myspell

firefox: myspell
	mkdir -p firefox ; tar jxvf firefox.tar.bz2 -C firefox
	cp LGPL firefox/license/license.txt
	cp wa.dic wa.aff firefox/dictionaries/
	perl -pe "s/\@\@VERSION\@\@/$(VERSION)/" -i firefox/*.*
	( cd firefox ; zip -r ../walloon_dictionary-$(VERSION)-fx+zm+tb.xpi * )

ispell_install: walon.tot
	install -m 644 walon.hash $(destdir)/usr/lib/ispell/
	install -m 644 walon.aff $(destdir)/usr/lib/ispell/

aspell_install: wa.rws
	#install -m 644 wa.multi $(destdir)/usr/lib/aspell-0.60/
	install -m 644 wa.dat $(destdir)/usr/lib/aspell-0.60/
	install -m 644 wa_phonet.dat $(destdir)/usr/lib/aspell-0.60/
	install -m 644 wa_affix.dat $(destdir)/usr/lib/aspell-0.60/
	install -m 644 wa.rws $(destdir)/usr/lib/aspell-0.60/
	install -m 644 wa.multi $(destdir)/usr/lib/aspell-0.60/
	#install -m 644 wa@*.rws $(destdir)/usr/lib/aspell-0.60/
	#install -m 644 wa@*.dat $(destdir)/usr/lib/aspell-0.60/

myspell_install: myspell
	install -m 644 wa.dic $(destdir)/usr/share/dict/ooo/
	install -m 644 wa.aff $(destdir)/usr/share/dict/ooo/

