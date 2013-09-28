# Created by: Adam Weinberger <adamw@FreeBSD.org>
# $FreeBSD: head/graphics/gimp-help/Makefile 327733 2013-09-20 18:35:44Z bapt $

PORTNAME=	gimp-help
PORTVERSION=	2.8.0
CATEGORIES=	graphics gnome
MASTER_SITES=	ftp://ftp.gimp.org/pub/%SUBDIR%/ \
		http://gimp.mirrors.hoobly.com/%SUBDIR%/ \
		http://ftp.gwdg.de/pub/misc/grafik/gimp/%SUBDIR%/ \
		http://www.mirrorservice.org/sites/ftp.gimp.org/pub/%SUBDIR%/ \
		${MASTER_SITE_RINGSERVER:S,%SUBDIR%,graphics/%SUBDIR%,}
MASTER_SITE_SUBDIR=	gimp/help/

MAINTAINER=	liangtai.s4@gmail.com
COMMENT=	GIMP User Manual

LICENSE=	GFDL

BUILD_DEPENDS+=	msgfmt:${PORTSDIR}/devel/gettext \
		xsltproc:${PORTSDIR}/textproc/libxslt \
		xmllint:${PORTSDIR}/textproc/libxml2
#	dblatex:${PORTSDIR}/textproc/dblatex \ # for build PDF
#	dot:${PORTSDIR}/graphics/graphviz \ # for build system devel. & debug
#	pngnq:${PORTSDIR}/graphics/pngnq \ # unused
#	pngcrush:${PORTSDIR}/graphics/pngcrush \ # unused
#RUN_DEPENDS=	gimp-2.8:${PORTSDIR}/graphics/gimp-app

USES=	gmake pkgconfig
USE_BZIP2=	yes
GNU_CONFIGURE=	yes

LANG_ALL=	de el en en_GB es fr it ja nl nn ru
LANG_ALMOST_UNTRANSLATED=	da fi hr ko lt pl sl sv zh_CN

# you can find more in ${PORTSDIR}/misc/kde4-l10n/files/kde4-lang-names
da_NAME=	Danish
de_NAME=	German
el_NAME=	Greek
en_NAME=	English
en_GB_NAME=	British English
es_NAME=	Spanish
fi_NAME=	Finnish
fr_NAME=	French
hr_NAME=	Croatian
it_NAME=	Italian
ja_NAME=	Japanese
ko_NAME=	Korean
lt_NAME=	Lithuanian
nl_NAME=	Dutch
nn_NAME=	Norwegian Nynorsk
pl_NAME=	Polish
ru_NAME=	Russian
sl_NAME=	Slovenian
sv_NAME=	Swedish
zh_CN_NAME=	Chinese Simplified

GIMPHELPDIR?=	${PREFIX}/share/gimp/help

.for lang in ${LANG_ALL}
${lang}_DETECT?=	${GIMPHELPDIR}/${lang}/index.html
${lang}_PORT?=		misc/gimp-help-${lang}
${lang}_NAME?=		${lang}
${lang:U}_DESC=		${${lang}_NAME}
.endfor

.MAKE.FreeBSD_UL=	yes
OPTIONS_DEFINE=	ALL
OPTIONS_DEFAULT=	ALL
ALL_DESC=	All translations
.for lang in ${LANG_ALL}
OPTIONS_DEFINE+=	${lang:U}
.endfor

NO_STAGE=	yes
.include <bsd.port.options.mk>

.for lang in ${LANG_ALL}
.if ${PORT_OPTIONS:MALL} || ${PORT_OPTIONS:M${lang:U}}
TARGETLANGS+=	${lang}
.endif
.endfor

do-build:
.for lang in ${TARGETLANGS}
	@cd ${WRKSRC} && ${GMAKE} html-${lang}
.endfor

do-install:
	@${MKDIR} ${STAGEDIR}/${GIMPHELPDIR}
.for lang in ${TARGETLANGS}
	@(cd ${WRKSRC}/html ; tar cLf - ${lang}) | \
	(cd ${STAGEDIR}/${GIMPHELPDIR} ; tar xvf -)
.endfor
	@${RM} -f ${WRKDIR}/PLIST.doc ; \
	${FIND} ${STAGEDIR}/${GIMPHELPDIR} -type f | \
		${SED} 's|${STAGEDIR}/${PREFIX}/||' \
		>> ${WRKDIR}/PLIST.doc ; \
	${FIND} ${STAGEDIR}/${GIMPHELPDIR} -type d | \
		${SED} 's|${STAGEDIR}/${PREFIX}/|@dirrm |' \
		| ${SORT} -r >> ${WRKDIR}/PLIST.doc ; \
	cd ${WRKDIR} ; ${SED} -i -e '/PLIST.doc/ r PLIST.doc' ${TMPPLIST}

.include <bsd.port.mk>
