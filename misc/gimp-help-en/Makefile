# Created by: SimaMoto,RyoTa <liangtai.s4@gmail.com>
# $FreeBSD$

PORTNAME=	gimp-help
PORTVERSION=	2.8.1
CATEGORIES?=	misc graphics
MASTER_SITES=	ftp://ftp.gimp.org/pub/%SUBDIR%/ \
		http://gimp.mirrors.hoobly.com/%SUBDIR%/ \
		http://ftp.gwdg.de/pub/misc/grafik/gimp/%SUBDIR%/ \
		http://www.mirrorservice.org/sites/ftp.gimp.org/pub/%SUBDIR%/ \
		${MASTER_SITE_RINGSERVER:S,%SUBDIR%,graphics/%SUBDIR%,}
MASTER_SITE_SUBDIR=	gimp/help
PKGNAMEPREFIX?=	${DOCLANG}-
PKGNAMESUFFIX=	-html

MAINTAINER=	liangtai.s16@gmail.com
COMMENT?=	GIMP User Manual in English

LICENSE=	GFDL

BUILD_DEPENDS+=	msgfmt:${PORTSDIR}/devel/gettext \
		xsltproc:${PORTSDIR}/textproc/libxslt \
		xmllint:${PORTSDIR}/textproc/libxml2
#RUN_DEPENDS=	gimp-2.8:${PORTSDIR}/graphics/gimp-app

USES=	gmake pkgconfig
USE_BZIP2=	yes
GNU_CONFIGURE=	yes

GIMPHELPDIR?=	${PREFIX}/share/gimp/help
DOCLANG?=	en
${DOCLANG}_DETECT?=	${GIMPHELPDIR}/${DOCLANG}/index.html

PLIST_FILES=	"@comment Insert PLIST.doc here"

do-build:
	cd ${WRKSRC} && ${GMAKE} html-${DOCLANG}

do-install:
	@${RM} -f ${WRKDIR}/PLIST.doc
	@${MKDIR} ${STAGEDIR}${GIMPHELPDIR}
	(cd ${WRKSRC}/html ; tar cLf - ${DOCLANG}) | \
	(cd ${STAGEDIR}${GIMPHELPDIR} ; tar xf -)
	@${FIND} ${STAGEDIR}${GIMPHELPDIR}/${DOCLANG} -type f | \
		${SED} 's|${STAGEDIR}${PREFIX}/||' \
		>> ${WRKDIR}/PLIST.doc ; \
	${FIND} ${STAGEDIR}${GIMPHELPDIR}/${DOCLANG} -type d | \
		${SED} 's|${STAGEDIR}${PREFIX}/|@dirrm |' \
		| ${SORT} -r >> ${WRKDIR}/PLIST.doc
	@cd ${WRKDIR} ; ${SED} -i -e '/PLIST.doc/ r PLIST.doc' ${TMPPLIST}

.include <bsd.port.mk>
