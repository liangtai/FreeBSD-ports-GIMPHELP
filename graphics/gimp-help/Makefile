# Created by: Adam Weinberger <adamw@FreeBSD.org>
# $FreeBSD: head/graphics/gimp-help/Makefile 327733 2013-09-20 18:35:44Z bapt $

PORTNAME=	gimp-help
PORTVERSION=	2.8.1
CATEGORIES=	graphics gnome
MASTER_SITES=	ftp://ftp.gimp.org/pub/%SUBDIR%/ \
		http://gimp.mirrors.hoobly.com/%SUBDIR%/ \
		http://ftp.gwdg.de/pub/misc/grafik/gimp/%SUBDIR%/ \
		http://www.mirrorservice.org/sites/ftp.gimp.org/pub/%SUBDIR%/ \
		${MASTER_SITE_RINGSERVER:S,%SUBDIR%,graphics/%SUBDIR%,}
MASTER_SITE_SUBDIR=	gimp/help/

MAINTAINER=	liangtai.s16@gmail.com
COMMENT=	GIMP User Manual

LICENSE=	GFDL

BUILD_DEPENDS+=	msgfmt:${PORTSDIR}/devel/gettext \
		xsltproc:${PORTSDIR}/textproc/libxslt \
		xmllint:${PORTSDIR}/textproc/libxml2
#RUN_DEPENDS=	gimp-2.8:${PORTSDIR}/graphics/gimp-app

USES=	gmake pkgconfig
USE_BZIP2=	yes
GNU_CONFIGURE=	yes

LANG_ALL_HTML=	ca da de el en en_GB es fr it ja ko nl nn pt_BR ru \
		sl sv zh_CN
# Translation come to 20%
LANG_RECOMMENDED_HTML=	de el en en_GB es fr it ja ko nn ru sl sv
LANG_ALL_QR= ca de el en fr it ja ko ru sv zh_CN
#LANG_ALL_PDF=	en
#LANG_ALL_ODF=	en

# you can find more in ${PORTSDIR}/misc/kde4-l10n/files/kde4-lang-names
ca_NAME=	Catalan
da_NAME=	Danish
de_NAME=	German
el_NAME=	Greek
en_NAME=	English
en_GB_NAME=	British English
es_NAME=	Spanish
fr_NAME=	French
it_NAME=	Italian
ja_NAME=	Japanese
ko_NAME=	Korean
nl_NAME=	Dutch
nn_NAME=	Norwegian Nynorsk
pt_BR_NAME=	Brazilian Portuguese
ru_NAME=	Russian
sl_NAME=	Slovenian
sv_NAME=	Swedish
zh_CN_NAME=	Chinese Simplified

OPTIONS_SUB=	yes
.for lang in ${LANG_ALL_HTML}
${lang}_DETECT?=	${GIMPHELPDIR}/${lang}/index.html
${lang}_NAME?=		${lang}
${lang:U}_HTML_DESC=		${${lang}_NAME}	online help
.endfor
ALL_HTML_DESC=	HTML for all translations (pkg-message shows stats)

ALL_QR_DESC=	Quick Reference Sheets
ALL_QR_DETECT?=	${DOCSDIR}/quickreference/gimp-keys-en.svg

#.for lang in ${LANG_ALL_PDF}
#${lang}_DETECT?=	${DOCSDIR}/pdf/${lang}.pdf
#${lang:U}_PDF_DESC=		${${lang}_NAME}	PDF handbook
#.endfor
#ALL_PDF_DESC=	PDF handbook for all translations on Latin
#.for lang in ${LANG_ALL_PDF} ALL
#${lang:U}_PDF_BUILD_DEPENDS+=	dblatex:${PORTSDIR}/textproc/dblatex
#.endfor

#.for lang in ${LANG_ALL_ODF}
#${lang}_DETECT?=	${DOCSDIR}/odf/${lang}.odt
#${lang:U}_ODF_DESC=		${${lang}_NAME}	ODF handbook
#.endfor
#ALL_ODF_DESC=	ODF handbook for all translations on Latin
#.for lang in ${LANG_ALL_ODF} ALL
#${lang:U}_ODF_BUILD_DEPENDS+=	docbook2odf:${PORTSDIR}/textproc/docbook2odf
#.endfor

.MAKE.FreeBSD_UL=	yes
OPTIONS_DEFINE=	ALL_HTML ALL_QR
.for lang in ${LANG_ALL_HTML}
OPTIONS_DEFINE+=	${lang:U}_HTML
.endfor
#.for lang in ${LANG_ALL_PDF}
#OPTIONS_DEFINE+=	${lang:U}_PDF
#.endfor
#.for lang in ${LANG_ALL_ODF}
#OPTIONS_DEFINE+=	${lang:U}_ODF
#.endfor

.for lang in ${LANG_RECOMMENDED_HTML}
OPTIONS_DEFAULT+=	${lang:U}_HTML
.endfor

.include <bsd.port.pre.mk>

GIMPHELPDIR?=	${PREFIX}/share/gimp/help
.for lang in ${LANG_ALL_HTML}
.if ${PORT_OPTIONS:MALL_HTML} || ${PORT_OPTIONS:M${lang:U}_HTML}
TARGETLANGS_HTML+=	${lang}
.endif
.endfor

.for lang in ${LANG_ALL_PDF}
.if ${PORT_OPTIONS:MALL_PDF} || ${PORT_OPTIONS:M${lang:U}_PDF}
TARGETLANGS_PDF+=	${lang}
.endif
.endfor

.for lang in ${LANG_ALL_ODF}
.if ${PORT_OPTIONS:MALL_ODF} || ${PORT_OPTIONS:M${lang:U}_ODF}
TARGETLANGS_ODF+=	${lang}
.endif
.endfor

pre-fetch:
	@${CAT} ${PKGMESSAGE}

do-build:
.for lang in ${TARGETLANGS_HTML}
	cd ${WRKSRC} && ${GMAKE} html-${lang}
.endfor
.for lang in ${TARGETLANGS_PDF}
	cd ${WRKSRC} && ${GMAKE} pdf-${lang}
.endfor
.for lang in ${TARGETLANGS_ODF}
	cd ${WRKSRC} && ${GMAKE} odf-${lang}
.endfor
.if ${PORT_OPTIONS:MALL_QR}
	cd ${WRKSRC} && ${GMAKE} -C quickreference
.endif

do-install:
	@${RM} -f ${WRKDIR}/PLIST.doc
	@${MKDIR} ${STAGEDIR}${GIMPHELPDIR}
.for lang in ${TARGETLANGS_HTML}
	(cd ${WRKSRC}/html ; tar cLf - ${lang}) | \
	(cd ${STAGEDIR}${GIMPHELPDIR} ; tar xf -)
.endfor
.for lang in ${TARGETLANGS_PDF}
	@${MKDIR} ${STAGEDIR}${DOCSDIR}/pdf
	${INSTALL_DATA} ${WRKSRC}/pdf/${lang}/gimp.pdf \
		${STAGEDIR}${DOCSDIR}/pdf/${lang}.pdf
	@${ECHO_CMD} ${DOCSDIR}/pdf/${lang}.pdf >> ${WRKDIR}/PLIST.doc
.endfor
.for lang in ${TARGETLANGS_ODF}
	@${MKDIR} ${STAGEDIR}${DOCSDIR}/odf
	${INSTALL_DATA} ${WRKSRC}/odf/${lang}/gimp.odt \
		${STAGEDIR}${DOCSDIR}/odf/${lang}.odt
	@${ECHO_CMD} ${DOCSDIR}/odf/${lang}.odt >> ${WRKDIR}/PLIST.doc
.endfor
.if ${PORT_OPTIONS:MALL_QR}
	@${MKDIR} ${STAGEDIR}${DOCSDIR}/quickreference
	${INSTALL_DATA} ${WRKSRC}/quickreference/svg/*.svg \
		${STAGEDIR}${DOCSDIR}/quickreference
.endif
.for lang in ${TARGETLANGS_HTML}
	@${FIND} ${STAGEDIR}${GIMPHELPDIR}/${lang} -type f | \
		${SED} 's|${STAGEDIR}${PREFIX}/||' \
		>> ${WRKDIR}/PLIST.doc ; \
	${FIND} ${STAGEDIR}${GIMPHELPDIR}/${lang} -type d | \
		${SED} 's|${STAGEDIR}${PREFIX}/|@dirrm |' \
		| ${SORT} -r >> ${WRKDIR}/PLIST.doc
.endfor
	@cd ${WRKDIR} ; ${SED} -i -e '/PLIST.doc/ r PLIST.doc' ${TMPPLIST}

.include <bsd.port.post.mk>
