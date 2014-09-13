An easy to configure OAI-PMH plugin which allows you to expose a number of domains as collections under an OAI-PMH service.


Add default config to config.groovy
defaultOaiConfig = [
  lastModified:'lastUpdated',
  schemas:[
    'oai_dc':[
      type:'method',
      methodName:'toOaiDcXml',
      schema:'http://www.openarchives.org/OAI/2.0/oai_dc.xsd',
      metadataNamespaces: [
        '_default_' : 'http://www.openarchives.org/OAI/2.0/oai_dc/',
        'dc'        : "http://purl.org/dc/elements/1.1/"
      ]],
  ]
]



Configure each domain class you want to be available via oai-pmh with a config block:


  @Transient
  static def oaiConfig = [
    id:'titles',   // URI prefix for this domain class
    textDescription:'Title repository for GOKb',
    query:" from TitleInstance as o where o.status.value != 'Deleted'",
    pageSize:20
  ]


And add the appropriate renderer classes to create OAI-DC and other metadata schema representations

  @Transient
  def toOaiDcXml(builder, attr) {
    builder.'dc'(attr) {
      'dc:title' (name)
    }
  }


