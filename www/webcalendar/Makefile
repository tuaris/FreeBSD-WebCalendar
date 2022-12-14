PORTNAME=	WebCalendar
DISTVERSIONPREFIX=	v
DISTVERSION=	1.9.1-40
DISTVERSIONSUFFIX=	-g6183409f
CATEGORIES=	www
PKGNAMESUFFIX=	${PHP_PKGNAMESUFFIX}
DISTNAME=	webcalendar-${DISTVERSIONPREFIX}${DISTVERSION}${DISTVERSIONSUFFIX}

MAINTAINER=	ports@FreeBSD.org
COMMENT=	Web-based calendar application
WWW=	http://www.k5n.us/webcalendar.php
LICENSE=	GPLv2

USES=		cpe php:web,flavors

USE_GITHUB=	yes
GH_ACCOUNT=	craigk5n
GH_PROJECT=	webcalendar

PORTSCOUT=	limit:^1\.2\. skipb:1
USE_PHP=	pcre session

NO_BUILD=	yes
NO_ARCH=	yes

CPE_VENDOR=	craig_knudsen
WWWDIR=		${PREFIX}/www/${PORTNAME:tl}
DOCSDIR=	${PREFIX}/share/doc/${PORTNAME:tl}

WITH_PHP_CGI?=	/cgi-bin/php
SUB_LIST+=	PHPCGI=${WITH_PHP_CGI}
PLIST_SUB+=	WWWOWN=${WWWOWN} WWWGRP=${WWWGRP}

PORTDOCS=	WebCalendar-SysAdmin.html newwin.gif

OPTIONS_DEFINE=		APACHE LDAP GRADIENTBG REMINDERS PALM DOCS EXAMPLES
OPTIONS_DEFAULT=	MYSQL REMINDERS
OPTIONS_MULTI=		DB
OPTIONS_MULTI_DB=	MYSQL PGSQL SQLITE MSSQL DBASE ODBC ORACLE
OPTIONS_SUB=		yes
GRADIENTBG_DESC=	Gradient background image support
REMINDERS_DESC=		Email reminder support
PALM_DESC=		Palm export support
DBASE_DESC=		Dbase support

APACHE_USE=	APACHE_RUN=24+
DBASE_USE=	PHP=dbase
GRADIENTBG_USE=	PHP=gd
LDAP_USE=	PHP=ldap
MSSQL_USE=	PHP=mssql
MYSQL_USE=	PHP=mysqli
ODBC_USE=	PHP=odbc
ORACLE_USE=	PHP=oracle
PALM_RUN_DEPENDS=	pilot-xfer:${PORTSDIR}/palm/pilot-link
PGSQL_USE=	PHP=pgsql
SQLITE_USE=	PHP=sqlite3

REMINDERS_USES=	php:cli

.include <bsd.port.pre.mk>

.if ${PORT_OPTIONS:MAPACHE}
PLIST_SUB+=	CONFDIR=${CONFDIR_REL}
CONFDIR=	${PREFIX}/${CONFDIR_REL}
CONFDIR_REL=	${APACHEETCDIR}/Includes
SUB_FILES=	pkg-message
.endif

SUB_FILES+=	webcalendar-cgi.conf webcalendar.conf

.if ${PHP_SAPI:Mcgi} == "cgi" && ${PHP_SAPI:Mmod} == ""
CGI_EXT=	-cgi
.else
CGI_EXT=
.endif

CONF=		webcalendar${CGI_EXT}.conf
PLIST_SUB+=	CONFFILE=${CONF}

.if ${PORT_OPTIONS:MDOCS}
SUB_LIST+=	HASHMARK=
.else
SUB_LIST+=	HASHMARK=\#
.endif

post-extract:
	@${RM} -rf ${WRKSRC}/.github ${WRKSRC}/docker

do-install:
	@${MKDIR} ${STAGEDIR}${WWWDIR}
	(cd ${WRKSRC} && ${COPYTREE_SHARE} . ${STAGEDIR}${WWWDIR})

post-install:
	@${MKDIR} ${STAGEDIR}${EXAMPLESDIR}
	${INSTALL_DATA} ${WRKDIR}/${CONF} ${STAGEDIR}${EXAMPLESDIR}

	@${MKDIR} ${STAGEDIR}${DOCSDIR}
	(cd ${WRKSRC}/docs && ${INSTALL_DATA} ${PORTDOCS} ${STAGEDIR}${DOCSDIR})

.include <bsd.port.post.mk>
