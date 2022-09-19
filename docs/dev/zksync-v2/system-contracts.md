# Понимание системных контрактов

Для того чтобы схемы zero-knowledge были максимально простыми и позволяли легко расширять их, большая часть логики zkSync была перенесена в так называемые "системные контракты" - набор контрактов, которые имеют особые привилегии и служат особым целям, например, развертывание контрактов, обеспечение того, чтобы пользователь платил только один раз за публикацию данных о контрактах и т.п.

До тех пор, пока код системных контрактов не пройдет тщательное тестирование, он не будет опубликован. Этот раздел только предоставит вам информацию, необходимую для разработки на zkSync.

## Интерфейсы

Адреса и интерфейсы системных контрактов могут быть найдены [тут.](https://github.com/matter-labs/v2-testnet-contracts/blob/main/l2/system-contracts/Constants.sol)

Этот раздел посвящен описанию семантического значения нескольких наиболее популярных системных контрактов.

## ContractDeployer

[Интерфейс](https://github.com/matter-labs/v2-testnet-contracts/blob/6a93ff85d33dfff0008624eb9777d5a07a26c55d/l2/system-contracts/interfaces/IContractDeployer.sol#L5)

Этот контракт используется для развертывания новых смарт-контрактов. Его задача - убедиться, что байт-код для каждого развернутого контракта известен. Этот контракт также определяет адрес получения. Всякий раз, когда контракт разворачивается, он вызывает событие `ContractDeployed`

В будущем мы добавим описание того, как напрямую взаимодействовать с этим контрактом.

## IL1Messenger

[Интерфейс](https://github.com/matter-labs/v2-testnet-contracts/blob/6a93ff85d33dfff0008624eb9777d5a07a26c55d/l2/system-contracts/interfaces/IL1Messenger.sol#L5)

Этот контракт используется для отправки сообщений с zkSync на Ethereum L1. Для каждого отправленного сообщения вызывается событие `L1MessageSent.`

## INonceHolder

[Интерфейс](https://github.com/matter-labs/v2-testnet-contracts/blob/6a93ff85d33dfff0008624eb9777d5a07a26c55d/l2/system-contracts/interfaces/INonceHolder.sol#L5)

Этот контракт хранит nonce'ы аккаунта. Nonce'ы аккаунта хранятся в одном месте ради эффективности ([nonce транзакции и nonce развертывания](https://v2-docs.zksync.io/dev/zksync-v2/contracts.html#differences-in-create-behaviour) хранятся в одном месте) и для упрощения работы оператора.

## Bootloader

For greater extensibility and to lower the overhead, some parts of the protocol (e.g. account abstraction rules) were moved to an ephemeral contract called _bootloader_. We call it ephemeral since formally it is never deployed and can not be called, but it has a formal [address](https://github.com/matter-labs/v2-testnet-contracts/blob/8de367778f3b7ed7e47ee8233c46c7fe046a75a3/l2/system-contracts/Constants.sol#L19) that is used on `msg.sender`, when it calls other contracts.

For now, you do not have to know any details about it, but knowing that it exists is important when you develop using the account abstraction feature. You can always assume that the bootloader is not malicious and it is a part of the protocol. In the future, the code of the bootloader will be made public and any changes to it will also mean an upgrade to the protocol.

