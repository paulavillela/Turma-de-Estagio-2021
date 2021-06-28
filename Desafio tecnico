package_ids = ['888555555123888', '333333555584333', '222333555124000', 
               '000111555874555', '111888555654777', '111333555123333', 
               '555555555123888', '888333555584333', '111333555124000', 
               '333888555584333', '555888555123000', '111888555123555', 
               '888000555845333', '000111555874000', '111333555123555']

regions_dict = {'111': 'Centro-oeste', '333': 'Nordeste',
               '555': 'Norte', '888': 'Sudeste', '000': 'Sul'}

product_dict = {'000': 'Jóias', '111': 'Livros', 
                     '333': 'Eletrônicos', '555': 'Bebidas',
                     '888': 'Brinquedos'}

loggi_id = '555'

inactive_cnpjs = ['584']

#Checa se o código do pacote é válido e, caso contrário, o motivo da invalidez
def package_restrictions():
    package_validation = []
    for id in package_ids:
        errors = []
        origin_id = id[0:3]
        if origin_id not in regions_dict:
            errors.append('Código da região de origem é inválido;')
        destination_id = id[3:6]
        if destination_id not in regions_dict:
            errors.append('Código da região de destino é inválido;')
        company_id = id[6:9]
        if company_id != loggi_id:
            errors.append('Código não é da Loggi;')
        vendor_id = id[9:12]
        if vendor_id in inactive_cnpjs:
            errors.append('CNPJ do vendedor está inativo;')
        product_id = id[12:15]
        if product_id not in product_dict:
            errors.append('Código do objeto é inválido;')
        package_validation.append(errors)
    return package_validation

#Cria e povoa o banco de dados dos pacotes
def populate_database(package_validation):
    database = []
    for id in package_ids:
        idx = package_ids.index(id)
        origin_id = id[0:3]
        if origin_id in regions_dict:
            origin = regions_dict[origin_id]
        else:
            origin = ''
        destination_id = id[3:6]
        if destination_id in regions_dict:
            destination = regions_dict[destination_id]
        else:
            destination = ''
        company_id = id[6:9]
        if company_id != loggi_id:
            company_id = 'Não é código Loggi'
        else:
            company_id = loggi_id
        vendor_id = id[9:12]
        product_type = id[12:15]
        if product_type in product_dict:
            product_type = product_dict[product_type]
        else:
            product_type = ''
        if len(package_validation[idx]) == 0:
            validation = 'Válido'
        else:
            validation = 'Inválido'
        package_dict = {'Código': id, 'Região de origem': origin,
                        'Região de destino': destination, 'Código Loggi': company_id,
                        'Código do vendedor do produto': vendor_id, 'Tipo do produto': product_type,
                        'Validez': validation}
        database.append(package_dict)
    return database

#Checa se há pacotes vindos do Centro-oeste que contém jóias, tornando-os inválidos
def center_west_jewels(database, package_validation):
    for package in database:
        if package['Região de origem'] == 'Centro-oeste' and package['Tipo do produto'] == 'Jóias':
            error = 'Pacote contém jóias e tem como região de origem Centro-oeste;'
            idx = database.index(package)
            package_validation[idx].append(error)
            package['Validez'] = 'Inválido'
    return database

# letra a) Identifica o destino de cada pacote
def identify_package_destination(database, package_id):
    for package in database:
        if package_id == package['Código']:
            print("Região de destino: ", package['Região de destino'])
            break

# letra b) Identifica se os pacotes tem código de barras válidos ou inválidos
def check_validation(database, package_validation):
    valid_packages = []
    invalid_packages = {}
    for package in database:
        if package['Validez'] == 'Válido':
            valid_packages.append(package['Código'])

        elif package['Validez'] == 'Inválido':
            package_id = package['Código']
            idx = database.index(package)
            invalid_packages[package_id] = idx

    print("\nPacotes válidos: ")
    for package in valid_packages:
        print(package)

    print("\nPacotes inválidos: ")
    for (package, index) in invalid_packages.items():
        print(package, ": ", package_validation[index])

#letra c) Identifica se algum pacote que tem como origem a região Sul tem brinquedos no seu código
def check_toys_south(database):
    packages = []
    for package in database:
        if package['Região de origem'] == 'Sul' and package['Tipo do produto'] == 'Brinquedos':
            packages.append(package['Código'])
    if(len(packages) == 0):
      print("Não há pacotes vindos do Sul com brinquedos.")
    else:
      print("Pacotes que vêm do Sul e tem brinquedos em seu conteúdo: ", packages)

#letra d) Lista os pacotes por região de destino
def packages_by_destination(database):
    north_packages = []
    northeast_packages = []
    midwest_packages = []
    southeast_packages = []
    south_packages = []
    for package in database:
        if package['Validez'] == 'Válido':
            if package['Região de destino'] == 'Norte':
                north_packages.append(package['Código'])

            elif package['Região de destino'] == 'Nordeste':
                northeast_packages.append(package['Código'])

            elif package['Região de destino'] == 'Centro-oeste':
                midwest_packages.append(package['Código'])

            elif package['Região de destino'] == 'Sudeste':
                southeast_packages.append(package['Código'])

            elif package['Região de destino'] == 'Sul':
                south_packages.append(package['Código'])
    print("\nPacotes com destino a região Norte: ", *north_packages, sep = '\n')
    print("\nPacotes com destino a região Nordeste: ", *northeast_packages, sep = '\n')
    print("\nPacotes com destino a região Centro-oeste: ", *midwest_packages, sep = '\n')
    print("\nPacotes com destino a região Sudeste: ", *southeast_packages, sep = '\n')
    print("\nPacotes com destino a região Sul: ", *south_packages, sep = '\n')

# e) Lista o número de pacotes enviados por cada vendedor
def packages_by_vendor(database):
    vendors = {}
    for package in database:
        if package['Validez'] == 'Válido':
            vendor_id = package['Código do vendedor do produto']
            if vendor_id not in vendors:
                vendors[vendor_id] = '1'
            else:
                quantity = int(vendors[vendor_id]) + 1
                vendors[vendor_id] = quantity
    
    for (vendor, packages) in vendors.items():
        print(vendor, ": ", packages)

# f) Lista os pacotes por destino e tipo
def order_destination_type(database):
    north_packages = {}
    northeast_packages = {}
    midwest_packages = {}
    southeast_packages = {}
    south_packages = {}
    for package in database:
        if package['Validez'] == 'Válido':
            if package['Região de destino'] == 'Norte':
                package_id = package['Código']
                product_type = package['Tipo do produto']
                north_packages[package_id] = product_type

            elif package['Região de destino'] == 'Nordeste':
                package_id = package['Código']
                product_type = package['Tipo do produto']
                northeast_packages[package_id] = product_type

            elif package['Região de destino'] == 'Centro-oeste':
                package_id = package['Código']
                product_type = package['Tipo do produto']
                midwest_packages[package_id] = product_type

            elif package['Região de destino'] == 'Sudeste':
                package_id = package['Código']
                product_type = package['Tipo do produto']
                southeast_packages[package_id] = product_type

            elif package['Região de destino'] == 'Sul':
                package_id = package['Código']
                product_type = package['Tipo do produto']
                south_packages[package_id] = product_type

    print("\nPacotes com região de destino Norte: ")
    for (id, type) in sorted(north_packages.items(), key=lambda x: x[1]):
        print(id, type)

    print("\nPacotes com região de destino Nordeste: ")
    for (id, type) in sorted(northeast_packages.items(), key=lambda x: x[1]):
        print(id, type)
        
    print("\nPacotes com região de destino Centro-oeste: ")
    for (id, type) in sorted(midwest_packages.items(), key=lambda x: x[1]):
        print(id, type)
        
    print("\nPacotes com região de destino Sudeste: ")
    for (id, type) in sorted(southeast_packages.items(), key=lambda x: x[1]):
        print(id, type)
        
    print("\nPacotes com região de destino Sul: ")
    for (id, type) in sorted(south_packages.items(), key=lambda x: x[1]):
        print(id, type)


def main():
    package_validation = package_restrictions()
    database = populate_database(package_validation)
    database = center_west_jewels(database, package_validation)

    choice = int(input("Escolha uma opção: \n1- Identificar o destino de um pacote. \n2- Pacotes válidos/inválidos. \n3- Identificar se algum pacote que vem do Sul possui brinquedos. \n4- Listar os pacotes agrupados por região de destino. \n5- Listar o número de pacotes enviados por vendedor. \n6- Listar pacotes por destino e tipo. \n0- Sair\nEscolha: "))
    while(choice != 0):
        if choice == 1:
            id = input("Código do pacote: ")
            identify_package_destination(database, id)
        elif choice == 2:
            check_validation(database, package_validation)
        elif choice == 3:
            check_toys_south(database)
        elif choice == 4:
            packages_by_destination(database)
        elif choice == 5:
            packages_by_vendor(database)
        elif choice == 6:
            order_destination_type(database)
        elif choice == 0:
            break
        else:
            print("\nEscolha inválida.")
        choice = int(input("\nEscolha uma opção: \n1- Identificar o destino de um pacote. \n2- Pacotes válidos/inválidos. \n3- Identificar se algum pacote que vem do Sul possui brinquedos. \n4- Listar os pacotes agrupados por região de destino. \n5- Listar o número de pacotes enviados por vendedor. \n6- Listar pacotes por destino e tipo. \n0- Sair\nEscolha: "))

main()
