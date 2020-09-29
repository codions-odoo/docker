FROM codions/odoo-base:12.0

WORKDIR /opt/odoo

ENV ODOO_VERSION 12.0

ENV PG_HOST localhost
ENV PG_PORT 5432
ENV PG_USER odoo
ENV PG_PASSWORD odoo
ENV PG_DATABASE False
ENV ODOO_PASSWORD senha_admin
ENV PORT 8069
ENV LOG_FILE /var/log/odoo/odoo.log
ENV LONGPOLLING_PORT 8072
ENV WORKERS 3
ENV DISABLE_LOGFILE 0
ENV USE_SPECIFIC_REPO 0
ENV TIME_CPU 600
ENV TIME_REAL 720

ARG CACHEBUST=1

RUN wget https://github.com/odoo/odoo/archive/${ODOO_VERSION}.zip -O odoo.zip && \
    wget https://github.com/oca/web/archive/${ODOO_VERSION}.zip -O web.zip && \
    wget https://github.com/oca/server-ux/archive/${ODOO_VERSION}.zip -O server-ux.zip && \
    wget https://github.com/oca/reporting-engine/archive/${ODOO_VERSION}.zip -O reporting-engine.zip && \
    wget https://github.com/oca/account-financial-reporting/archive/${ODOO_VERSION}.zip -O account-financial-reporting.zip && \
    wget https://github.com/oca/mis-builder/archive/${ODOO_VERSION}.zip -O mis-builder.zip && \
    # Custom
    wget https://github.com/Trust-Code/odoo-brasil/archive/${ODOO_VERSION}.zip -O odoo-brasil.zip && \
    wget https://github.com/Trust-Code/trustcode-addons/archive/${ODOO_VERSION}.zip -O trustcode-addons.zip

RUN unzip -q odoo.zip && rm odoo.zip && mv odoo-${ODOO_VERSION} odoo && \
    unzip -q web.zip && rm web.zip && mv web-${ODOO_VERSION} web && \
    unzip -q server-ux.zip && rm server-ux.zip && mv server-ux-${ODOO_VERSION} server-ux && \
    unzip -q reporting-engine.zip && rm reporting-engine.zip && mv reporting-engine-${ODOO_VERSION} reporting-engine && \
    unzip -q account-financial-reporting.zip && rm account-financial-reporting.zip && mv account-financial-reporting-${ODOO_VERSION} account-financial-reporting && \
    unzip -q mis-builder.zip && rm mis-builder.zip && mv mis-builder-${ODOO_VERSION} mis-builder && \
    # Odoo Brazil
    unzip -q odoo-brasil.zip && rm odoo-brasil.zip && mv odoo-brasil-${ODOO_VERSION} odoo-brasil && \
    unzip -q trustcode-addons.zip && rm trustcode-addons.zip && mv trustcode-addons-${ODOO_VERSION} trustcode-addons

RUN cd odoo && find . -name "*.po" -not -name "pt_BR.po" -not -name "pt.po"  -type f -delete && \
    find . -path "*l10n_*" -delete && \
    rm -R debian && rm -R doc && rm -R setup && cd ..

RUN pip install --no-cache-dir pytrustnfe3 python3-cnab python3-boleto pycnab240 python-sped

ADD conf/odoo.conf /etc/odoo/
RUN chown -R odoo:odoo /opt && \
    chown -R odoo:odoo /etc/odoo/odoo.conf

ADD bin/autoupdate /opt/odoo
ADD bin/entrypoint.sh /opt/odoo
RUN chown odoo:odoo /opt/odoo/autoupdate && \
    chmod +x /opt/odoo/autoupdate && \
    chmod +x /opt/odoo/entrypoint.sh

WORKDIR /opt/odoo

VOLUME ["/opt/", "/etc/odoo"]
ENTRYPOINT ["/opt/odoo/entrypoint.sh"]
CMD ["/usr/bin/supervisord"]
