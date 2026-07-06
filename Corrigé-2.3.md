## Corrigé 2.3 — ORM SQLAlchemy 2.0

```python
from typing import List
from sqlalchemy import create_engine, String, ForeignKey, select
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column, relationship, Session


class Base(DeclarativeBase):
    pass


class Equipement(Base):
    __tablename__ = "equipement"
    id: Mapped[int] = mapped_column(primary_key=True)
    hostname: Mapped[str] = mapped_column(String(50))
    interfaces: Mapped[List["Interface"]] = relationship(back_populates="equipement")


class Interface(Base):
    __tablename__ = "interface"
    id: Mapped[int] = mapped_column(primary_key=True)
    nom: Mapped[str] = mapped_column(String(50))
    equipement_id: Mapped[int] = mapped_column(ForeignKey("equipement.id"))
    equipement: Mapped["Equipement"] = relationship(back_populates="interfaces")


engine = create_engine("sqlite:///:memory:")
Base.metadata.create_all(engine)

with Session(engine) as session:
    sw = Equipement(hostname="sw-access-02")
    sw.interfaces = [Interface(nom="GigabitEthernet0/1"), Interface(nom="GigabitEthernet0/2")]
    session.add(sw)
    session.commit()

    for e in session.scalars(select(Equipement)):
        noms = [i.nom for i in e.interfaces]
        print(f"{e.hostname} -> {noms}")
```

**Sortie attendue :**
```text
sw-access-02 -> ['GigabitEthernet0/1', 'GigabitEthernet0/2']
```

**Points clés :** la relation bidirectionnelle (`relationship` + `back_populates`) permet d'affecter directement `sw.interfaces = [...]` ; en ajoutant l'équipement à la session, SQLAlchemy insère aussi les interfaces et renseigne automatiquement la clé étrangère `equipement_id`. `select(Equipement)` + `session.scalars(...)` renvoie les objets.