# Created by: SimaMoto,RyoTa <liangtai.s4@gmail.com>
# $FreeBSD$

CATEGORIES=	russian graphics

MAINTAINER=	liangtai.s16@gmail.com
COMMENT=	The GIMP User Manual in Russian

LICENSE=	GFDL

MASTERDIR=	${.CURDIR}/../../misc/gimp-help-en

DOCLANG=	ru

post-patch:
	${REINPLACE_CMD} -e '/glossary\.xml:1596/{; n; n; s/xref/link/g; }' \
		${WRKSRC}/po/ru/glossary.po

.include "${MASTERDIR}/Makefile"
